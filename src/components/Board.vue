<template>
  <div>
    <div className="board-row">
      <square :value="squares[0]" @click.stop="onClick(0)" />
      <square :value="squares[1]" @click.stop="onClick(1)" />
      <square :value="squares[2]" @click.stop="onClick(2)" />
    </div>
    <div className="board-row">
      <square :value="squares[3]" @click.stop="onClick(3)" />
      <square :value="squares[4]" @click.stop="onClick(4)" />
      <square :value="squares[5]" @click.stop="onClick(5)" />
    </div>
    <div className="board-row">
      <square :value="squares[6]" @click.stop="onClick(6)" />
      <square :value="squares[7]" @click.stop="onClick(7)" />
      <square :value="squares[8]" @click.stop="onClick(8)" />
    </div>
  </div>
</template>

<script>
import Square from "./Square.vue";

export default {
  components: {
    Square,
  },
  props: {
    squares: Array,
  },
  methods: {
    onClick(i) {
      this.$emit("click", i);
    },
  },
};
</script>
