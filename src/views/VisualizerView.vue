<template>
  <div class="header">Task Assignment Visualizer</div>
  <div class="display">
    <canvas ref="display" id="task-display" width="400" height="400"></canvas>
  </div>
  <div class="inputs"></div>
</template>

<script>
import { ref, onMounted } from "vue";

export default {
  name: "VisualizerView",
  setup() {
    const display = ref(null);
    const tasks = [
      {
        name: "a",
        cost: 10,
        location: { x: 300, y: 100 },
      },
      {
        name: "b",
        cost: 10,
        location: { x: 300, y: 150 },
      },
      {
        name: "c",
        cost: 10,
        location: { x: 300, y: 200 },
      },
      {
        name: "d",
        cost: 10,
        location: { x: 300, y: 250 },
      },
    ];

    const agents = {
      Agent1: {
        name: "Agent1",
        location: {
          x: 100,
          y: 100,
        },
      },
      Agent2: {
        name: "Agent2",
        location: {
          x: 20,
          y: 110,
        },
      },
      Agent3: {
        name: "Agent3",
        location: {
          x: 160,
          y: 200,
        },
      },
      Agent4: {
        name: "Agent4",
        location: {
          x: 170,
          y: 250,
        },
      },
      Agent5: {
        name: "Agent5",
        location: {
          x: 270,
          y: 90,
        },
      },
    };

    function draw(ctx, agents, tasks) {
      ctx.lineWidth = 2;
      ctx.strokeStyle = "red";

      tasks.forEach((task) => {
        ctx.beginPath();
        ctx.arc(
          task.location.x,
          task.location.y,
          task.cost,
          0,
          Math.PI * 2,
          true
        );
        ctx.stroke();
      });

      ctx.fillStyle = "green";
      Object.values(agents).forEach((agent) => {
        ctx.beginPath();
        ctx.fillRect(agent.location.x - 10, agent.location.y - 10, 20, 20);
      });
    }

    onMounted(() => {
      let ctx = display.value.getContext("2d");
      draw(ctx, agents, tasks);
    });

    return {
      display,
    };
  },
};
</script>

<style scoped></style>
