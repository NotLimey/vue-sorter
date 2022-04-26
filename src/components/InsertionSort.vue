
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
      let arr = this.data;
      const n = arr.length;
      let i, key, j; 
      for (i = 1; i < n; i++)
      { 
          key = arr[i]; 
          j = i - 1; 
    
          /* Move elements of arr[0..i-1], that are 
          greater than key, to one position ahead 
          of their current position */
          while (j >= 0 && arr[j] > key)
          { 
              arr[j + 1] = arr[j]; 
              j = j - 1; 
          } 
          arr[j + 1] = key; 
          this.active = j;
          await this.sleep(5)
          this.data = arr;
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
  <div class="flex gap-1 justify-center items-center mt-14">
    <div v-for="(item, index) in data" v-bind:key="item">
      <div
        class="rounded-full"
        :class="(index === active || index + 1 === active) ? 'bg-green-400' : index % 2 === 0 ? 'bg-red-500' : 'bg-red-200'"
        :style="'width: ' + 2 + 'px;' + 'height: ' + item * 10 + 'px;'"
      ></div>
    </div>
  </div>
</template>
