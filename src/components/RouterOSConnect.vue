<template>
  <div>
    <!-- BaseModal for RouterOS connection -->
    <BaseModal @close="$emit('close', 'dashboard')">
      <h2 class="text-xl font-bold mb-4">Connect to RouterOS</h2>
      <form @submit.prevent="connectRouter">
        <input
          v-model="form.ip_address"
          placeholder="IP Address"
          class="input"
        />
        <input v-model="form.login" placeholder="Login" class="input" />
        <input
          type="password"
          v-model="form.password"
          placeholder="Password"
          class="input"
        />
        <button class="btn" type="submit">Connect</button>
      </form>
    </BaseModal>

    <!-- RouterOS Connection Information -->
  </div>
</template>
<script setup>
import { ref } from "vue";
import api from "../axios"; // Axios API instance
import BaseModal from "./BaseModal.vue"; // Modal component

// 👇 Define emits
const emit = defineEmits(["connectedData", "close"]);

// Reactive form and router data state
const form = ref({ ip_address: "", login: "", password: "" });
const routeros_data = ref([]);

// Connect RouterOS API call
const connectRouter = async () => {
  try {
    const res = await api.post("/routeros-connect", form.value);
    routeros_data.value = res.data;

    // 👇 Emit the connected data to the parent
    emit("connectedData", routeros_data.value);
    emit("close", "dashboard");
  } catch (error) {
    console.error("Error connecting to RouterOS:", error);
  }
};
</script>


<style scoped>
/* Styling for inputs and buttons */
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
  cursor: pointer;
}
</style>
