<template>
  <div class="cell" @click="changeState">
    <!-- <span>{{ symbols[state] }}</span> -->
    <img class="shadow" :src="symbols[state]" />
  </div>
</template>

<script>
export default {
  data: () => ({
    symbols: [
      "",
      require("@/assets/cross.svg"),
      require("@/assets/circle.svg")
    ],
    state: 0,
    enable: true
  }),
  computed: {},
  props: {
    index: Number,
    turn: Boolean
  },
  methods: {
    changeState() {
      if (this.enable) {
        this.enable = false;
        this.state = this.turn ? 1 : 2;
        this.$emit("nextTurn", this.index);
      }
    }
  }
};
</script>

<style>
.cell {
  border: 1px solid black;
  width: 15vw;
  height: 15vw;
  text-align: center;
  font-size: 8vw;
  background: rgb(255, 255, 255);
}

.cell:hover {
  opacity: 0.8;
  cursor: pointer;
}

img {
  transform: scale(0.7);
}

.shadow {
  filter: drop-shadow(3px 3px 2px rgba(0, 0, 0, 0.7));
}
</style>
