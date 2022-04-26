
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
    sleep(milliseconds) {
      return new Promise((resolve) => setTimeout(resolve, milliseconds));
    },
    async sort() {
      let inputArr = this.data;
      let n = inputArr.length;
      for (let i = 1; i < n; i++) {
        // Choosing the first element in our unsorted subarray
        let current = inputArr[i];
        // The last element of our sorted subarray
        let j = i - 1;
        while (j > -1 && current < inputArr[j]) {
          inputArr[j + 1] = inputArr[j];
          j--;
          this.data = inputArr;
        }
        this.active = i;
        inputArr[j + 1] = current;
        this.sleep(50);
      }
      this.active = -2;
      return inputArr;
    },
  },
  mounted() {
    this.data = this.randomArray(0, 10);
  },
};
</script>


<template>
  <div class="flex justify-center items-center mt-14">
    <div v-for="(item, index) in data" v-bind:key="item">
      <div
        class="rounded-full"
        :class="index === active ? 'bg-green-400' : index % 2 === 0 ? 'bg-red-500' : 'bg-red-200'"
        :style="'width: ' + 1 + 'px;' + 'height: ' + item * 10 + 'px;'"
      ></div>
    </div>
  </div>
</template>
