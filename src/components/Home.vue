<template>
  <div>
    <div style="text-align:center; margin-top:150px">
      <canvas
        ref="game"
        width="650"
        height="480"
        style="border:1px solid black"
      ></canvas>
      <div style="margin-top:50px">
        <button @click="move('left')">
          <i class="lni lni-arrow-left"></i>
        </button>
        <button @click="move('up')"><i class="lni lni-arrow-up"></i></button>
        <button @click="move('right')">
          <i class="lni lni-arrow-right"></i>
        </button>
        <div>
          <button @click="move('down')">
            <i class="lni lni-arrow-down"></i>
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import io from "socket.io-client";
export default {
  data() {
    return {
      socket: {},
      context: {},
      position: {
        x: 0,
        y: 0,
      },
    };
  },
  created() {
    this.socket = io("http://localhost:3000/");
  },
  mounted() {
    this.context = this.$refs.game.getContext("2d");
    this.socket.on("position", (data) => {
      this.position = data;
      this.context.clearRect(
        0,
        0,
        this.$refs.game.width,
        this.$refs.game.height
      );
      this.context.fillRect(this.position.x, this.position.y, 20, 20);
    });
  },
  methods: {
    move(direction) {
      this.socket.emit("move", direction);
    },
  },
};
</script>
