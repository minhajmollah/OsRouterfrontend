<template>
  <BaseModal @close="$emit('close')">
    <h2 class="text-xl font-bold mb-4">Add DNS Server</h2>
    <form @submit.prevent="submit">
      <input
        v-model="form.dns_server"
        placeholder="DNS Server IP"
        class="input"
      />
      <button class="btn" type="submit">Add DNS</button>
    </form>
    <pre v-if="response">{{ response }}</pre>
  </BaseModal>
</template>

<script setup>
import { ref } from "vue";
import api from "../axios";
import BaseModal from "./BaseModal.vue";

const form = ref({ dns_server: "" });
const response = ref(null);

const submit = async () => {
  const res = await api.post("/add_dns_servers", form.value);
  response.value = res.data;
};
</script>
