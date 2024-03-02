<template>
  <div class="container">
    <div
      v-for="(number, index) in numsArray"
      :key="index"
      :style="{
        height: 5 + number.number + '%',
        width: 100 / numsArray.length + 'vh',
      }"
      :class="[active(index, number)]"
    >
      {{ this.numsArray.length < 40 ? number.number : null }}
    </div>
  </div>
</template>

<script>
export default {
  name: "NumsArray",
  props: {
    numsArray: Array,
    value_i: Number,
    value_j: Number,
    value_largest: Number,
    pivot: Number,
    done: Boolean,
    doneGreen: Boolean,
    turnRed: Boolean,
    counter: Number,
    sortType: String,
    finalArray: Array,
  },
  methods: {
    active(index, number) {
      if (this.sortType === "Bubble Sort" || this.sortType === "") {
        if (this.doneGreen) {
          return "active";
        }
        if (this.done) {
          return "purple";
        }
        if (this.counter <= index && this.counter >= 0) {
          return "purple";
        } else if (this.value_i === index || this.value_j === index) {
          if (this.turnRed) {
            return "red";
          }
          return "active";
        } else {
          return "element";
        }
      } else if (this.sortType === "Merge Sort") {
        if (this.doneGreen) {
          return "active";
        }
        if (this.done) {
          return "purple";
        }
        if (this.value_i === number.id || this.value_j === number.id) {
          if (this.turnRed) {
            return "red";
          }
          return "active";
        } else {
          return "element";
        }
      } else if (this.sortType === "Quick Sort") {
        if (this.doneGreen) {
          return "active";
        }
        if (this.done) {
          return "purple";
        }

        if (this.pivot === index) {
          if (
            (this.value_i === this.pivot || this.value_j === this.pivot) &&
            this.turnRed
          ) {
            return "red";
          }
          if (this.value_i === this.pivot || this.value_j === this.pivot) {
            return "active";
          }
          if (
            (this.turnRed && this.value_i === this.pivot) ||
            (this.turnRed && this.value_j === this.pivot)
          ) {
            return "red";
          }
          return "orange";
        }

        if (this.value_i === index || this.value_j === index) {
          if (this.turnRed) {
            return "red";
          }
          return "active";
        } else {
          if (
            this.numsArray[index] === this.finalArray[index] &&
            this.counter >= 0
          ) {
            return "purple";
          }
          return "element";
        }
      } else if (this.sortType === "Heap Sort") {
        if (this.doneGreen) {
          return "active";
        }
        if (this.done) {
          return "purple";
        }
        if (this.counter <= index && this.counter >= 0) {
          return "purple";
        }

        if (
          this.value_i === index ||
          this.value_j === index ||
          this.value_largest === index
        ) {
          if (this.turnRed && this.value_largest !== -1) {
            if (this.value_largest === index || this.value_i === index) {
              return "red";
            }
            return "active";
          } else if (this.turnRed && this.value_largest === -1) {
            if (this.value_j === index || this.value_i === index) {
              return "red";
            }
            return "active";
          }
          return "active";
        } else {
          return "element";
        }
      }
    },
  },
};
</script>

<style scoped>
.orange {
  margin: 0.3%;
  background-color: rgb(241, 146, 111);
}
.purple {
  margin: 0.3%;
  background-color: purple;
}
.red {
  margin: 0.3%;
  background-color: rgb(247, 35, 35);
}
.active {
  margin: 0.3%;
  background-color: green;
}
.element {
  width: 20px;
  background-color: rgb(65, 65, 236);
  margin: 0.3%;
  height: 12%;
}
.container {
  height: 50vh;
  max-width: 60%;
  min-width: 260px;
  margin: auto;
  display: flex;
  justify-content: center;
  color: white;
  text-align: center;

  font-size: 1.1vw;
}
</style>
