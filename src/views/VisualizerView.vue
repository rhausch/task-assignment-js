<template>
  <div class="header">Task Assignment Visualizer</div>
  <div class="display">
    <canvas ref="display" id="task-display" width="400" height="400"></canvas>
  </div>
  <div class="inputs"></div>
</template>

<script>
import { ref, onMounted, onUnmounted } from "vue";

export default {
  name: "VisualizerView",
  setup() {
    const display = ref(null);
    const tasks = {
      a: {
        name: "a",
        cost: 10,
        location: { x: 300, y: 100 },
      },
      b: {
        name: "b",
        cost: 10,
        location: { x: 300, y: 150 },
      },
      c: {
        name: "c",
        cost: 10,
        location: { x: 300, y: 200 },
      },
      d: {
        name: "d",
        cost: 10,
        location: { x: 300, y: 250 },
      },
    };

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

    let assignments = null;
    let animationHandle = null;
    let ctx = null;

    function draw(ctx, agents, tasks) {
      ctx.clearRect(0, 0, display.value.width, display.value.height);

      ctx.lineWidth = 2;
      ctx.strokeStyle = "red";

      Object.values(tasks).forEach((task) => {
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

    function assignTasks(agents, tasks) {
      let assignments = [];
      Object.values(agents).forEach((agent) => {
        let closest = 99999;
        let bestTask = null;
        Object.values(tasks).forEach((task) => {
          let dist = distanceSqrd(
            agent.location.x,
            agent.location.y,
            task.location.x,
            task.location.y
          );
          if (dist < closest) {
            closest = dist;
            bestTask = task.name;
          }
        });
        assignments.push({ agent: agent.name, task: bestTask });
      });
      return assignments;
    }

    function gameStep(agents, tasks, assignments) {
      assignments.forEach((assignment) => {
        agents[assignment.agent].location = moveTowards(
          agents[assignment.agent].location,
          tasks[assignment.task].location
        );
      });
    }

    function moveTowards(locA, locB) {
      let x = locB.x - locA.x;
      let y = locB.y - locA.y;
      if ((x === 0) & (y === 0)) return locA;
      let h = Math.sqrt(x * x + y * y);
      return { x: locA.x + x / h, y: locA.y + y / h };
    }

    function distanceSqrd(x1, y1, x2, y2) {
      return (x1 - x2) * (x1 - x2) + (y1 - y2) * (y1 - y2);
    }

    function runGameLoop() {
      animationHandle = requestAnimationFrame(runGameLoop);

      if (!display.value) return;

      assignments = assignTasks(agents, tasks);
      console.log(agents);
      gameStep(agents, tasks, assignments);
      draw(ctx, agents, tasks);
    }

    onMounted(() => {
      ctx = display.value.getContext("2d");
      runGameLoop();
    });

    onUnmounted(() => {
      cancelAnimationFrame(animationHandle);
    });

    return {
      display,
    };
  },
};
</script>

<style scoped></style>
