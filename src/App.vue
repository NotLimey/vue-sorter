
<script>
import BubbleSorter from "./components/BubbleSorter.vue";
import SelectionSort from "./components/SelectionSort.vue";
import InsertionSort from "./components/InsertionSort.vue";
import SettingsComponent from "./components/SettingsComponent.vue";
const tabs = [
  { name: "BubbleSorter", component: BubbleSorter },
  { name: "SelectionSort", component: SelectionSort },
  { name: "InsertionSort", component: InsertionSort },
];

export default {
  components: { BubbleSorter, SelectionSort, InsertionSort, SettingsComponent },
  data() {
    return {
      tabs: tabs,
      currentTab: tabs[0].name,
      clicked: false,
      settingsOpen: false,
      settings: {},
      componentKey: 0
    };
  },
  methods: {
    sortData() {
      this.clicked = true;
      this.$refs.componentRef.sort();
    },
    scrambleData() {
      this.$refs.componentRef.scramble();
    },
    changeTab(tab) {
      if (this.currentTab !== tab.name) {
        this.currentTab = tab.name;
        this.clicked = false;
        this.updateTab();
      }
    },
    updateTab() {
      window.localStorage.setItem(
        "currentTab",
        JSON.stringify(this.currentTab)
      );
    },
    closeSettings() {
      window.document.querySelector("body").classList.remove("stop-scrolling");
      const set = window.localStorage.getItem("settings")
      this.settings = JSON.parse(set)
      this.settingsOpen = false;
      window.location.reload();
    },
    openSettings() {
      this.settingsOpen = true;
    },
  },
  mounted() {
    const currentTab = window.localStorage.getItem("currentTab");
    const settings = window.localStorage.getItem("settings");
    if (!settings) {
      const _settings = {
        amount: 30,
      };
      window.localStorage.setItem("settings", JSON.stringify(_settings));
      this.settings = {
        amount: 30,
      };
    } else {
      const set = window.localStorage.getItem("settings")
      this.settings = JSON.parse(set)
    }
    if (currentTab) {
      this.currentTab = JSON.parse(currentTab);
    }
  }
};
</script>

<template>
  <settings-component :key="componentKey" v-if="settingsOpen" @close="closeSettings()" />
  <div class="px-10">
    <div class="relative pb-5 border-b border-gray-700 sm:pb-0 mt-10">
      <div class="md:flex md:items-center md:justify-between">
        <h3 class="text-lg leading-6 font-medium text-stone-100">Sorting</h3>
        <div class="mt-3 flex md:mt-0 md:absolute md:top-3 md:right-0">
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
              text-white
              bg-stone-800
              hover:bg-stone-700
              focus:outline-none
              focus:ring-2
              focus:ring-offset-2
              focus:ring-green-500
            "
            @click="openSettings()"
          >
            Settings
          </button>
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
              text-white
              bg-stone-800
              hover:bg-stone-700
              focus:outline-none
              focus:ring-2
              focus:ring-offset-2
              focus:ring-green-500
            "
            @click="scrambleData()"
          >
            Scramble
          </button>
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
            :disabled="this.clicked"
            @click="sortData()"
          >
            Sort
          </button>
        </div>
      </div>
      <div class="mt-4">
        <div class="sm:hidden">
          <label for="current-tab" class="sr-only">Select a tab</label>
          <select
            id="current-tab"
            name="current-tab"
            class="
              block
              w-full
              pl-3
              pr-10
              py-2
              text-base
              border-stone-800
              focus:outline-none focus:ring-green-500 focus:border-green-500
              sm:text-sm
              rounded-md
            "
          >
            <option v-for="tab in tabs" :key="tab.name" :selected="currentTab">
              {{ tab.name }}
            </option>
          </select>
        </div>
        <div class="hidden sm:block">
          <nav class="-mb-px flex space-x-8">
            <button
              v-for="tab in tabs"
              :key="tab.name"
              :class="[
                tab.name === currentTab
                  ? 'border-green-500 text-green-600'
                  : 'border-transparent text-stone-400 hover:text-stone-300 hover:border-stone-600',
                'whitespace-nowrap pb-4 px-1 border-b-2 font-medium text-sm',
              ]"
              :aria-current="tab.name === currentTab ? 'page' : undefined"
              @click="changeTab(tab)"
            >
              {{ tab.name }}
            </button>
          </nav>
        </div>
      </div>
    </div>
    <keep-alive>
      <component :is="currentTab" ref="componentRef" :settings="settings" />
    </keep-alive>
  </div>
</template>

<style>
@import "./assets/base.css";
</style>
