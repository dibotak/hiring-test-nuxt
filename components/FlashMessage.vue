<template>
  <div class="fixed top-0 right-0 m-6">
    <div
      v-if="message"
      :class="{
        'bg-red-200 text-red-900': message.type === 'error',
        'bg-green-200 text-green-900': message.type === 'success',
      }"
      class="rounded-lg shadow-md p-6 pr-10"
      style="min-width: 240px"
    >
      <button
        class="opacity-75 cursor-pointer absolute top-0 right-0 py-2 px-3 hover:opacity-100"
        @click="onClose"
      >
        <svg class="fill-current h-6 w-6 text-red-500" role="button" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 20 20"><title>Close</title><path d="M14.348 14.849a1.2 1.2 0 0 1-1.697 0L10 11.819l-2.651 3.029a1.2 1.2 0 1 1-1.697-1.697l2.758-3.15-2.759-3.152a1.2 1.2 0 1 1 1.697-1.697L10 8.183l2.651-3.031a1.2 1.2 0 1 1 1.697 1.697l-2.758 3.152 2.758 3.15a1.2 1.2 0 0 1 0 1.698z"/></svg>
      </button>
      <div class="flex items-center">
        {{ message.text }}
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: ['message', 'onClose'],
};
</script>
