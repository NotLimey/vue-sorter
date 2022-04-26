
<script>
import { XIcon } from "@heroicons/vue/outline";

export default {
  components: { XIcon },
  mounted() {
    window.document.querySelector("body").classList.add("stop-scrolling");
  },
};
</script>


<template>
  <div
    class="
      w-screen
      h-screen
      fixed
      top-0
      left-0
      z-10
      backdrop-blur-xl
      bg-stone-500 bg-opacity-40
      flex
      items-center
      justify-center
    "
  >
    <div class="bg-stone-900 w-96 p-10 max-h-full h-96 rounded-lg shadow-xl">
      <div class="flex items-center justify-between gap-x-2">
        <h2 class="text-xl">Settings</h2>
        <XIcon class="w-6 h-6" @click="$emit('close')" />
      </div>
    </div>
  </div>
</template>