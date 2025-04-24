<template>
  <BaseModal @close="$emit('close', 'dashboard')">
    <h2 class="text-xl font-bold mb-4">Add IP Route</h2>
    <form @submit.prevent="submit">
      <input v-model="form.ip_address" placeholder="Ip Address" class="input" />

      <input v-model="form.gateway" placeholder="Gateway" class="input" />
      <button class="btn" type="submit">Add Route</button>
    </form>
    <pre v-if="response">{{ response }}</pre>
  </BaseModal>
</template>

<script setup>
import { ref } from "vue";
import api from "../axios";
import BaseModal from "./BaseModal.vue";

const form = ref({ gateway: "", ip_address: "" });
const routeros_data = ref([]);
const emit = defineEmits(["connectedData", "close"]);

const submit = async () => {
  const res = await api.post("/add-ip-route", form.value);
  routeros_data.value = res.data;

  // 👇 Emit the connected data to the parent
  emit("connectedData", routeros_data.value);
  emit("close", "dashboard");
};
</script>
