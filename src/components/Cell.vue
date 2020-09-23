<template>
  <div class="cell" @click="changeState">
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
  props: {
    index: Number,
    turn: Boolean,
    winner: Boolean
  },
  methods: {
    changeState() {
      // If there is a winner, then you cant click anymore on the cell
      if (this.winner === true) {
        return;
      } else if (this.enable) {
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
  background: #dedede;
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
