<template>
  <BaseModal @close="$emit('close', 'dashboard')">
    <h2 class="text-xl font-bold mb-4">Set Interface</h2>
    <form @submit.prevent="submit">
      <input v-model="form.ip_address" placeholder="Ip Address" class="input" />
      <input v-model="form.name" placeholder="Interface Name" class="input" />
      <input v-model="form.id" placeholder="id" class="input" />
      <input
        v-model="form.interface"
        placeholder="Which interface want to set"
        class="input"
      />
      <button class="btn" type="submit">Set Interface</button>
    </form>
  </BaseModal>
</template>

<script setup>
import { ref } from "vue";
import api from "../axios";
import BaseModal from "./BaseModal.vue";
const emit = defineEmits(["connectedData", "close"]);
const form = ref({ name: "", id: "", interface: "", ip_address: "" });
const routeros_data = ref([]);

const submit = async () => {
  const res = await api.post("/set-interface", form.value);
  routeros_data.value = res.data;
  emit("connectedData", routeros_data.value);
  emit("close", "dashboard");
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
