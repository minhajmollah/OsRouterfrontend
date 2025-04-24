<template>
  <BaseModal @close="$emit('close', 'dashboard')">
    <h2 class="text-xl font-bold mb-4">Add New Address</h2>
    <form @submit.prevent="submit">
      <input v-model="form.interface" placeholder="Interface" class="input" />
      <input
        v-model="form.ip_address"
        placeholder="IP Address/CIDR"
        class="input"
      />
      <input v-model="form.address" placeholder="Address" class="input" />
      <button class="btn" type="submit">Add Address</button>
    </form>
    <pre v-if="response">{{ response }}</pre>
  </BaseModal>
</template>

<script setup>
import { ref } from "vue";
import api from "../axios";
import BaseModal from "./BaseModal.vue";

const form = ref({ interface: "", address: "", ip_address: "" });
const routeros_data = ref([]);
const emit = defineEmits(["connectedData", "close"]);

const submit = async () => {
  const res = await api.post("/add-new-address", form.value);
  routeros_data.value = res.data;
  emit("connectedData", routeros_data.value);
  emit("close", "dashboard");
};
</script>
