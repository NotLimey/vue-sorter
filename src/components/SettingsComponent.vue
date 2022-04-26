
<script>
import { XIcon } from "@heroicons/vue/outline";

export default {
  components: { XIcon },
  mounted() {
    window.document.querySelector("body").classList.add("stop-scrolling");
    const settings = window.localStorage.getItem("settings");
    if (settings) {
      this.settings = JSON.parse(settings);
    }
  },
  data() {
    return {
      settings: {},
    };
  },
  methods: {
    save() {
      window.localStorage.setItem("settings", JSON.stringify(this.settings));
      this.$emit("close");
    },
  },
};
</script>


<template>
  <div
    ref="settings"
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
    <div
      class="
        bg-stone-900
        w-96
        px-10
        py-6
        max-h-full
        h-96
        rounded-lg
        shadow-xl
        flex flex-col
        justify-between
      "
    >
      <div class="flex items-center justify-between gap-x-2">
        <h2 class="text-xl">Settings</h2>
        <XIcon class="w-6 h-6 cursor-pointer" @click="$emit('close')" />
      </div>
      <div class="h-full">
        <div class="flex gap-2 my-4">
          <div class="flex items-center">
            <label htmlFor="text w-full"> Data amount: </label>
            <input
              type="number"
              name="text"
              id="text"
              class="
                bg-stone-800
                ml-5
                border-none
                outline-hidden outline-0
                focus:ring-0
                w-1/3
                text-stone-200
                rounded-md
              "
              :value="settings.amount"
              @input="(e) => (settings.amount = e.target.value)"
              placeholder=""
            />
          </div>
        </div>
      </div>
      <div class="flex justify-end">
        <button
          type="button"
          class="
            ml-3
            inline-flex
            items-center
            px-4
            py-2
            border border-transparent
            rounded-md
            shadow-sm
            text-sm
            font-medium
            text-black
            bg-green-600
            hover:bg-green-700
            focus:outline-none
            focus:ring-2
            focus:ring-offset-2
            focus:ring-green-500
          "
          @click="save()"
        >
          Save
        </button>
      </div>
    </div>
  </div>
</template>