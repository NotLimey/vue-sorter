
<script>
export default {
  data() {
    return {
      data: [],
      active: -2,
      amount: 100
    };
  },
  methods: {
    randomArray(min, max) {
      let set = window.localStorage.getItem("settings")
      set = JSON.parse(set) || {amount: 100}
      var arr = [];
      for (var i = 0; i < set.amount; i++) {
        arr.push(min + Math.random() * (max - min));
      }
      this.amount = set.amount
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
          await this.sleep(0);
        }
      }
      this.active = -2;
      this.$emit("unDisable");
    },
  },
  mounted() {
    this.data = this.randomArray(0, 15);
  },
};
</script>


<template>
  <div class="flex gap-1 justify-center items-end mt-14 w-full">
    <div v-for="(item, index) in data" v-bind:key="item" :style="'width: ' + (100 / amount).toFixed(2) + '%;'  ">
      <div
        class="rounded-[2px]"
        :class="(index === active || index + 1 === active )? 'bg-green-400' : index % 2 === 0 ? 'bg-red-500' : 'bg-red-200'"
        :style="'width: 100%;' + 'height: ' + item * 30 + 'px;'"
      ></div>
    </div>
  </div>
</template>
