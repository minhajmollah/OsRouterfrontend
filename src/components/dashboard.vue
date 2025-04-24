<template>
  <div
    class="p-6 max-w-4xl mx-auto bg-white rounded-xl shadow-md space-y-6"
    @close="handleClose"
  >
    <div
      class="text-xl font-bold text-gray-800 flex justify-between items-center"
    >
      <span>RouterOS Connection</span>
      <span class="text-green-600 font-medium">Connected</span>
    </div>

    <!-- RouterOS Data -->
    <div v-if="routeros_data.routeros_data" class="text-gray-700 space-y-2">
      <p>
        <strong>Identity:</strong> {{ routeros_data.routeros_data.identity }}
      </p>
      <p>
        <strong>IP Address:</strong>
        {{ routeros_data.routeros_data.ip_address }}
      </p>
      <p><strong>Login:</strong> {{ routeros_data.routeros_data.login }}</p>
    </div>

    <!-- Total connected devices -->
    <div class="text-sm text-gray-600" v-if="routeros_data.routeros_data">
      <strong>Total Connected Devices:</strong>
      {{ routeros_data ? routeros_data.active_users?.length || 0 : 0 }}
    </div>

    <div
      class="p-4 bg-white shadow rounded-lg space-y-3"
      v-if="routeros_data[0]"
    >
      <h2 class="text-lg font-bold text-gray-800">RouterOS</h2>
      <h3 class="text-md font-semibold text-green-600">Add DNS Server</h3>
      <p class="text-sm text-gray-700">
        <strong>Message:</strong> Successfully addedd new dns servers
      </p>

      <div class="mt-4 text-sm text-gray-800">
        <h4 class="font-semibold mb-2">DNS List:</h4>
        <ul class="list-disc pl-5">
          <li v-for="(value, key) in routeros_data[0]" :key="key">
            <strong>{{ key.replace(/-/g, " ") }}:</strong> {{ value || "—" }}
          </li>
        </ul>
      </div>
    </div>
    <div>
      <h2
        class="text-lg font-semibold text-gray-800 mb-4"
        v-if="routeros_data.routeros_data"
      >
        Interfaces
      </h2>

      <div v-if="routeros_data.routeros_data">
        <table class="min-w-full bg-white shadow-md rounded-lg overflow-hidden">
          <thead class="bg-gray-200 text-gray-700 text-sm uppercase text-left">
            <tr>
              <th class="px-4 py-2">Name</th>
              <th class="px-4 py-2">Type</th>
              <th class="px-4 py-2">MAC Address</th>
              <th class="px-4 py-2">Status</th>
              <th class="px-4 py-2">RX (Bytes)</th>
              <th class="px-4 py-2">TX (Bytes)</th>
              <th class="px-4 py-2">RX Packets</th>
              <th class="px-4 py-2">TX Packets</th>
              <th class="px-4 py-2">Comment</th>
            </tr>
          </thead>
          <tbody>
            <tr
              v-for="(iface, index) in routeros_data.routeros_data.traffic"
              :key="index"
              class="border-t text-sm text-gray-800 hover:bg-gray-50"
            >
              <td class="px-4 py-2 font-medium">{{ iface.name }}</td>
              <td class="px-4 py-2">{{ iface.type }}</td>
              <td class="px-4 py-2">{{ iface["mac-address"] }}</td>
              <td class="px-4 py-2">
                {{ iface.running === "true" ? "Running" : "Stopped" }}
              </td>
              <td class="px-4 py-2">{{ iface["rx-byte"] }}</td>
              <td class="px-4 py-2">{{ iface["tx-byte"] }}</td>
              <td class="px-4 py-2">{{ iface["rx-packet"] }}</td>
              <td class="px-4 py-2">{{ iface["tx-packet"] }}</td>
              <td class="px-4 py-2">{{ iface.comment || "—" }}</td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>

    <div class="p-6" v-if="routeros_data.route_lists">
      <h1 class="text-xl font-bold text-green-600 mb-4">
        {{ routeros_data.message }}
      </h1>

      <h2 class="text-lg font-semibold mb-2">Route Lists</h2>
      <table class="table-auto w-full border border-gray-300">
        <thead class="bg-gray-100">
          <tr>
            <th class="border px-4 py-2">#</th>
            <th class="border px-4 py-2">.id</th>
            <th class="border px-4 py-2">Destination</th>
            <th class="border px-4 py-2">Gateway</th>
            <th class="border px-4 py-2">Pref Src</th>
            <th class="border px-4 py-2">Active</th>
            <th class="border px-4 py-2">Static</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(route, index) in routeros_data.route_lists" :key="index">
            <td class="border px-4 py-2">{{ index + 1 }}</td>
            <td class="border px-4 py-2">{{ route[".id"] }}</td>
            <td class="border px-4 py-2">{{ route["dst-address"] }}</td>
            <td class="border px-4 py-2">{{ route.gateway }}</td>
            <td class="border px-4 py-2">{{ route["pref-src"] || "-" }}</td>
            <td class="border px-4 py-2">{{ route.active }}</td>
            <td class="border px-4 py-2">{{ route.static }}</td>
          </tr>
        </tbody>
      </table>
    </div>
    <div v-if="message" class="mt-4 text-green-600">
      <strong>{{ routeros_data.message }}</strong>
    </div>

    <ul
      v-if="routeros_data?.address_lists?.length"
      class="mt-4 border p-4 rounded bg-gray-100"
    >
      <li
        v-for="(item, index) in routeros_data.address_lists"
        :key="index"
        class="mb-2"
      >
        <strong>ID:</strong> {{ item[".id"] }} <br />
        <strong>Address:</strong> {{ item.address }} <br />
        <strong>Network:</strong> {{ item.network }} <br />
        <strong>Interface:</strong> {{ item.interface }}
      </li>
    </ul>
    <div v-if="routeros_data" class="mt-4">
      <p class="text-green-600 font-semibold">{{ routeros_data.message }}</p>

      <table class="table-auto w-full mt-4 border">
        <thead class="bg-gray-200">
          <tr>
            <th class="border px-2 py-1">Name</th>
            <th class="border px-2 py-1">Type</th>
            <th class="border px-2 py-1">MAC Address</th>
            <th class="border px-2 py-1">RX Byte</th>
            <th class="border px-2 py-1">TX Byte</th>
            <th class="border px-2 py-1">Running</th>
            <th class="border px-2 py-1">Disabled</th>
            <th class="border px-2 py-1">Comment</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="item in routeros_data.interface_lists" :key="item['.id']">
            <td class="border px-2 py-1">{{ item.name }}</td>
            <td class="border px-2 py-1">{{ item.type }}</td>
            <td class="border px-2 py-1">{{ item["mac-address"] }}</td>
            <td class="border px-2 py-1">{{ item["rx-byte"] }}</td>
            <td class="border px-2 py-1">{{ item["tx-byte"] }}</td>
            <td class="border px-2 py-1">{{ item.running }}</td>
            <td class="border px-2 py-1">{{ item.disabled }}</td>
            <td class="border px-2 py-1">{{ item.comment || "-" }}</td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>
<script setup>
import { ref } from "vue";
defineProps({
  routeros_data: Array,
});
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