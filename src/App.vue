<template>
  <div class="flex min-h-screen">
    <!-- Sidebar -->
    <aside class="w-64 bg-gray-800 text-white p-4">
      <h2 class="text-2xl font-bold mb-4">RouterOS</h2>
      <ul>
        <li v-for="item in menu" :key="item.label" class="mb-2">
          <button
            @click="activeComponent = item.key"
            class="w-full text-left hover:bg-gray-700 p-2 rounded"
          >
            {{ item.label }}
          </button>
        </li>
      </ul>
    </aside>

    <!-- Main -->
    <main class="flex-1 p-6">
      <component
        :is="activeComponent"
        @close="activeComponent = null"
        v-if="activeComponent"
      />
    </main>
  </div>
</template>

<script setup>
import { ref } from "vue";
import RouterOSConnect from "./components/RouterOSConnect.vue";
import SetInterface from "./components/SetInterface.vue";
import AddNewAddress from "./components/AddNewAddress.vue";
import AddIpRoute from "./components/AddIpRoute.vue";
import AddDnsServers from "./components/AddDnsServers.vue";

const activeComponent = ref(null);

const menu = [
  { label: "Connect to RouterOS", key: RouterOSConnect },
  { label: "Set Interface", key: SetInterface },
  { label: "Add New Address", key: AddNewAddress },
  { label: "Add IP Route", key: AddIpRoute },
  { label: "Add DNS Servers", key: AddDnsServers },
];
</script>
