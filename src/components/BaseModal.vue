<template>
  <div>
    <div
      class="fixed inset-0 bg-light w-full p-4 w-80 bg-opacity-50 flex items-center justify-center z-50"
      style="
        position: fixed;
        top: 50%;
        left: 50%;
        transform: translate(-50%, -50%);
      "
    >
      <div
        class="bg-white p-6 rounded-lg w-full max-w-lg shadow-lg relative"
        style="padding: 50px"
      >
        <button
          class="absolute top-2 right-2 text-gray-500"
          @click="$emit('close')"
        >
          ✕
        </button>
        <slot />
      </div>
    </div>
  </div>
</template>
