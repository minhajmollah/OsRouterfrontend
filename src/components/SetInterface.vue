<template>
  <BaseModal @close="$emit('close')">
    <h2 class="text-xl font-bold mb-4">Set Interface</h2>
    <form @submit.prevent="submit">
      <input v-model="form.name" placeholder="Interface Name" class="input" />
      <input v-model="form.type" placeholder="Type" class="input" />
      <button class="btn" type="submit">Set Interface</button>
    </form>
    <pre v-if="response">{{ response }}</pre>
  </BaseModal>
</template>

<script setup>
import { ref } from "vue";
import api from "../axios";
import BaseModal from "./BaseModal.vue";

const form = ref({ name: "", type: "" });
const response = ref(null);

const submit = async () => {
  const res = await api.post("/set_interface", form.value);
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
