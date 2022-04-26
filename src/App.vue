
<script>
import { faker } from "@faker-js/faker";
export default {
  data() {
    return {
      data: [],
      active: -2,
    };
  },
  methods: {
    randomArray(min, max) {
      var arr = [];
      for(var i = 0; i < 10; i++) {
        arr.push((min + Math.random() * (max - min)).toFixed());
      }
      return arr;
    },
    sleep(milliseconds) {
      return new Promise((resolve) => setTimeout(resolve, milliseconds));
    },
    async sort(arr) {
      for (var i = 0; i < arr.length; i++) {
        for (var j = 0; j < arr.length - i - 1; j++) {
          // Checking if the item at present iteration
          // is greater than the next iteration
          if (arr[j] > arr[j + 1]) {
            // If the condition is true then swap them
            var temp = arr[j];
            arr[j] = arr[j + 1];
            arr[j + 1] = temp;
          }
          this.active = j;
          await this.sleep(200);
          this.data = arr;
        }
      }
      this.active = null;
    },
  },
  mounted() {
    this.data = this.randomArray(0, 20);
    console.log(this.randomArray(0, 20))
  },
};
</script>


<template>
  <div class="flex gap-4 justify-center items-center mt-14">
    <div v-for="(item, index) in data" v-bind:key="item">
      <div
        class="rounded-full"
        :class="
          active === index || active + 1 === index
            ? 'bg-green-400'
            : 'bg-red-500'
        "
        :style="'width: ' + item * 8 + 'px;' + 'height: ' + item * 8 + 'px;'"
      ></div>
    </div>
  </div>
  <button @click="sort(this.data)">Sort</button>
</template>

<style>
@import "./assets/base.css";
</style>
