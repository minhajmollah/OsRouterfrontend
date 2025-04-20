<template>
  <BaseModal @close="$emit('close')">
    <h2 class="text-xl font-bold mb-4">Connect to RouterOS</h2>
    <form @submit.prevent="connectRouter">
      <input v-model="form.ip_address" placeholder="IP Address" class="input" />
      <input v-model="form.login" placeholder="Login" class="input" />
      <input
        type="password"
        v-model="form.password"
        placeholder="Password"
        class="input"
      />
      <button class="btn" type="submit">Connect</button>
    </form>
    <pre v-if="response">{{ response }}</pre>
  </BaseModal>
</template>

<script setup>
import { ref } from "vue";
import api from "../axios";
import BaseModal from "./BaseModal.vue";

const form = ref({ ip_address: "", login: "", password: "" });
const response = ref(null);

const connectRouter = async () => {
  const res = await api.post("/routeros-connection", form.value);
  response.value = res.data;
};
</script>

<style scoped>
.input {
  display: block;
  width: 100%;
  margin-bottom: 10px;
  padding: 8px;
  border: 1px solid #ccc;
}
.btn {
  background: #3b82f6;
  color: white;
  padding: 8px 16px;
  border-radius: 5px;
}
</style>
