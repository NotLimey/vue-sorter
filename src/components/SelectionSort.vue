
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
      let set = window.localStorage.getItem("settings")
      set = JSON.parse(set)
      var arr = [];
      for (var i = 0; i < set.amount; i++) {
        arr.push(min + Math.random() * (max - min));
      }
      const uniqueList = [...new Set(arr)];
      return uniqueList;
    },
    scramble() {
      this.data = this.randomArray(0, 15);
    },
    sleep(milliseconds) {
      return new Promise((resolve) => setTimeout(resolve, milliseconds));
    },
    async sort() {
      let inputArr = this.data;
      let n = inputArr.length;

      for (let i = 0; i < n; i++) {
        // Finding the smallest number in the subarray
        let min = i;
        for (let j = i + 1; j < n; j++) {
          if (inputArr[j] < inputArr[min]) {
            min = j;
          }
          this.data = inputArr;
          this.active = i;
          await this.sleep(1);
        }
        if (min != i) {
          // Swapping the elements
          let tmp = inputArr[i];
          inputArr[i] = inputArr[min];
          inputArr[min] = tmp;
        }
      }
      this.active = -2;
      return inputArr;
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
        :class="index === active ? 'bg-green-400' : index % 2 === 0 ? 'bg-red-500' : 'bg-red-200'"
        :style="'width: ' + 2 + 'px;' + 'height: ' + item * 10 + 'px;'"
      ></div>
    </div>
  </div>
</template>
