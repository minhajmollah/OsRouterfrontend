<template>
  <BaseModal @close="$emit('close')">
    <h2 class="text-xl font-bold mb-4">Add IP Route</h2>
    <form @submit.prevent="submit">
      <input
        v-model="form.dst_address"
        placeholder="Destination Address"
        class="input"
      />
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

const form = ref({ dst_address: "", gateway: "" });
const response = ref(null);

const submit = async () => {
  const res = await api.post("/add_ip_route", form.value);
  response.value = res.data;
};
</script>
