
<script>
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
      for (var i = 0; i < 30; i++) {
        arr.push(min + Math.random() * (max - min));
      }
      const uniqueList = [...new Set(arr)];
      return uniqueList;
    },
    sleep(milliseconds) {
      return new Promise((resolve) => setTimeout(resolve, milliseconds));
    },
    async sort() {
      let arr = this.data;
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
          this.data = arr;
          await this.sleep();
        }
      }
      this.active = -2;
    },
  },
  mounted() {
    this.data = this.randomArray(0, 15);
  },
};
</script>


<template>
  <div class="flex gap-1 justify-center items-center mt-14">
    <div v-for="(item, index) in data" v-bind:key="item">
      <div
        class="rounded-full"
        :class="index % 2 === 0 ? 'bg-green-400' : 'bg-red-500'"
        :style="'width: ' + 2 + 'px;' + 'height: ' + item * 10 + 'px;'"
      ></div>
    </div>
  </div>
</template>
