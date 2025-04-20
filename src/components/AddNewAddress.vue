<template>
  <BaseModal @close="$emit('close')">
    <h2 class="text-xl font-bold mb-4">Add New Address</h2>
    <form @submit.prevent="submit">
      <input v-model="form.interface" placeholder="Interface" class="input" />
      <input
        v-model="form.address"
        placeholder="IP Address/CIDR"
        class="input"
      />
      <button class="btn" type="submit">Add Address</button>
    </form>
    <pre v-if="response">{{ response }}</pre>
  </BaseModal>
</template>

<script setup>
import { ref } from "vue";
import api from "../axios";
import BaseModal from "./BaseModal.vue";

const form = ref({ interface: "", address: "" });
const response = ref(null);

const submit = async () => {
  const res = await api.post("/add_new_address", form.value);
  response.value = res.data;
};
</script>
