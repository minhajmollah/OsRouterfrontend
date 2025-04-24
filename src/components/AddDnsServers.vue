<template>
  <BaseModal @close="$emit('close', 'dashboard')">
    <h2 class="text-xl font-bold mb-4">Add DNS Server</h2>
    <form @submit.prevent="submit">
      <input v-model="form.servers" placeholder="DNS Server IP" class="input" />
      <input v-model="form.ip_address" placeholder="Ip Address" class="input" />
      <button class="btn" type="submit">Add DNS</button>
    </form>
    <pre v-if="response">{{ response }}</pre>
  </BaseModal>
</template>

<script setup>
import { ref } from "vue";
import api from "../axios";
import BaseModal from "./BaseModal.vue";
const emit = defineEmits(["connectedData", "close"]);
const form = ref({ dns_server: "", remote_requests: "yes", ip_address: "" });
const routeros_data = ref([]);

const submit = async () => {
  const res = await api.post("/add-dns-server", form.value);
  routeros_data.value = res.data.dns_lists;
  emit("connectedData", routeros_data.value);
  emit("close", "dashboard");
};
</script>
