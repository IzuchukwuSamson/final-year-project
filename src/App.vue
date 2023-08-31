<template>
  <div class="container">
    <h2>SPRINKLER IRRIGATION MANAGEMENT APPLICATION</h2>

    <div class="right-container">
      <div class="gantt-selected-info">
        <div v-if="selectedTask">
          <h2>{{ selectedTask.text }}</h2>
          <span><b>ID: </b>{{ selectedTask.id }}</span
          ><br />
          <span><b>Progress: </b> {{ progressPercentage }}</span
          ><br />
          <span><b>Start Date: </b>{{ formattedStartDate }}</span
          ><br />
          <span><b>End Date: </b>{{ formattedEndDate }}</span
          ><br />
          <span v-for="task in selectedTask.info" :key="task"
            ><b>Info </b> <br />
            <b> {{ task.first }} </b> <br />
            <b>{{ task.second }} </b> <br />
            <b> {{ task.third }} </b> <br />
          </span>
        </div>
        <div v-else class="select-task-prompt">
          <h2>Click any task</h2>
        </div>
      </div>
      <ul class="gantt-messages">
        <li
          class="gantt-message"
          v-for="(message, index) in messages"
          v-bind:key="index"
        >
          {{ message }}
        </li>
      </ul>
    </div>
    <GanttComponent
      class="left-container"
      :tasks="tasks"
      @task-updated="logTaskUpdate"
      @link-updated="logLinkUpdate"
      @task-selected="selectTask"
    ></GanttComponent>
  </div>
</template>

<script>
import GanttComponent from "./components/GanttComponent.vue";

export default {
  name: "app",
  components: { GanttComponent },
  data() {
    return {
      tasks: {
        data: [
          {
            id: 1,
            text: "Site Accessmennt",
            start_date: "2023-06-17",
            duration: 3,
            progress: 0.6,
            info: [
              {
                first:
                  "Topographic Infomation: Average elevation of 500 meters above sea level, Gentle slopes with an average gradient of 2%",
                second:
                  "Soil Analysis: Loamy soil with a balanced mixture of sand, silt, and clay, High porosity with water holding capacity of 25%, Nitrogen (N) - 40 ppm, Phosphorus (P) - 25 ppm, Potassium (K) - 200 ppm",
                third:
                  "Climate Assessment: Annual rainfall of 800 mm, 8-month growing season, from April to November.",
              },
            ],
          },
          {
            id: 2,
            text: "Crop and Water Requirement Analysis",
            start_date: "2023-06-18",
            duration: 3,
            progress: 0.4,
            info: [
              {
                first: "Water Requirement = ET × Kc + Effective Rainfal",
                second:
                  "Water Requirement = (5.5 mm/day × 0.2) + (7.5 mm/day × 1.0) + (5.5 mm/day × 0.7) + 800 mm = 1460 mm + 750 mm + 385 mm + 800 mm = 3395 mm",
                third: "",
              },
            ],
          },
          // {
          //   id: 3,
          //   text: "System Selection",
          //   start_date: "2023-06-19",
          //   duration: 3,
          //   // progress: 0.4,
          //   info: [],
          // },
          {
            id: 4,
            text: "Layout Design:",
            start_date: "2023-06-17",
            duration: 3,
            progress: 0.4,
            info: [
              {
                first:
                  "Number of Blocks: Divide the land into four blocks, each covering approximately 1.5 hectares.",
                second:
                  "Block Arrangement: Arrange the blocks in a staggered pattern, considering the land's natural contours",
              },
            ],
          },
          {
            id: 5,
            text: "Hydraulic Design:",
            start_date: "2023-06-21",
            duration: 3,
            progress: 0.4,
            info: [
              {
                first:
                  "Main Canal Flow: Estimate the main canal flow rate based on the crop's water requirement and the number of blocks. For example, if each block requires 1500 mm of water and there are four blocks, the total water requirement is 6000 mm. Considering the growing season length of 8 months, the average monthly flow rate needed would be 750 mm.",
                second:
                  "Canal Dimensions: Design the main canal with a width of 1.5 meters and a depth of 0.6 meters, providing a cross-sectional area of 0.9 square meters.",
                third:
                  "Secondary Ditch Flow: Distribute the flow to each block through secondary ditches. If the width of a secondary ditch is 0.4 meters and the depth is 0.3 meters, the cross-sectional area would be 0.12 square meters.",
              },
            ],
          },
          {
            id: 6,
            text: "Material Procurement:",
            start_date: "2023-06-18",
            duration: 3,
            progress: 0.4,
            info: [
              {
                first:
                  "Concrete: Estimate the quantity of concrete required for constructing the main canal based on its dimensions (width, depth, length).",
                second:
                  "Reinforcement Bars: Procure steel reinforcement bars for reinforcing the concrete structure.",
                // third:
                //   "Formwork: Acquire formwork materials for shaping and containing the concrete during pouring.",
              },
            ],
          },
          {
            id: 7,
            text: "Permitting and Approvals:",
            start_date: "2023-06-19",
            duration: 3,
            progress: 0.4,
            info: [
              {
                first:
                  "Land Use and Zoning Permit: Application Fees: $200 (estimated) and Processing Time: 1 month",
                second:
                  "Environmental Impact Assessment (EIA) Clearance: Consultancy Fees: $1500 (for an environmental consultant) EIA Report Preparation: $2000 (includes studies and documentation) ",
              },
            ],
          },
          {
            id: 8,
            text: "Clearing and Grading:",
            start_date: "2023-06-21",
            duration: 3,
            progress: 0.4,
            info: [],
          },
          {
            id: 9,
            text: "Pipework Installation:",
            start_date: "2023-06-20",
            duration: 3,
            progress: 0.4,
            info: [],
          },
          {
            id: 10,
            text: "Sprinkler Installation:",
            start_date: "2023-06-22",
            duration: 3,
            progress: 0.4,
            info: [],
          },
          {
            id: 11,
            text: "Valve and Control Installation:",
            start_date: "2023-06-23",
            duration: 3,
            progress: 0.4,
            info: [],
          },
          {
            id: 12,
            text: "Pump Installation:",
            start_date: "2023-06-24",
            duration: 3,
            progress: 0.4,
            info: [],
          },
          {
            id: 13,
            text: "System Testing:",
            start_date: "2023-06-20",
            duration: 3,
            progress: 0.4,
            info: [],
          },
          {
            id: 14,
            text: "Adjustments:",
            start_date: "2023-06-19",
            duration: 3,
            progress: 0.4,
            info: [],
          },
          {
            id: 15,
            text: "Safety Checks:",
            start_date: "2023-06-18",
            duration: 3,
            progress: 0.4,
            info: [],
          },
          {
            id: 16,
            text: "Maintenance:",
            start_date: "2023-06-16",
            duration: 3,
            progress: 0.4,
            info: [],
          },

          {
            id: 17,
            text: "Monitoring and Optimization:",
            start_date: "2023-06-17",
            duration: 3,
            progress: 0.4,
            info: [],
          },
          // {
          //   id: 18,
          //   text: "Layout Design:",
          //   start_date: "2023-06-20",
          //   duration: 3,
          //   progress: 0.4,
          //   info: [],
          // },
          // {
          //   id: 19,
          //   text: "Layout Design:",
          //   start_date: "2023-06-20",
          //   duration: 3,
          //   progress: 0.4,
          //   info: "lorem ipsum",
          // },
          // {
          //   id: 20,
          //   text: "Layout Design:",
          //   start_date: "2023-06-20",
          //   duration: 3,
          //   progress: 0.4,
          //   info: "lorem ipsum",
          // },
          // {
          //   id: 21,
          //   text: "Layout Design:",
          //   start_date: "2023-06-20",
          //   duration: 3,
          //   progress: 0.4,
          //   info: "lorem ipsum",
          // },
          // {
          //   id: 22,
          //   text: "Layout Design:",
          //   start_date: "2023-06-20",
          //   duration: 3,
          //   progress: 0.4,
          //   info: "lorem ipsum",
          // },
          // {
          //   id: 23,
          //   text: "Layout Design:",
          //   start_date: "2023-06-20",
          //   duration: 3,
          //   progress: 0.4,
          //   info: "lorem ipsum",
          // },
          // {
          //   id: 24,
          //   text: "Layout Design:",
          //   start_date: "2023-06-20",
          //   duration: 3,
          //   progress: 0.4,
          //   info: "lorem ipsum",
          // },
          // {
          //   id: 25,
          //   text: "Layout Design:",
          //   start_date: "2023-06-20",
          //   duration: 3,
          //   progress: 0.4,
          //   info: "lorem ipsum",
          // },
          // {
          //   id: 26,
          //   text: "Layout Design:",
          //   start_date: "2023-06-20",
          //   duration: 3,
          //   progress: 0.4,
          //   info: "lorem ipsum",
          // },
          // {
          //   id: 27,
          //   text: "Layout Design:",
          //   start_date: "2023-06-20",
          //   duration: 3,
          //   progress: 0.4,
          //   info: "lorem ipsum",
          // },
          // {
          //   id: 28,
          //   text: "Layout Design:",
          //   start_date: "2023-06-20",
          //   duration: 3,
          //   progress: 0.4,
          //   info: "lorem ipsum",
          // },
          // {
          //   id: 29,
          //   text: "Layout Design:",
          //   start_date: "2023-06-20",
          //   duration: 3,
          //   progress: 0.4,
          //   info: "lorem ipsum",
          // },
          // {
          //   id: 30,
          //   text: "Layout Design:",
          //   start_date: "2023-06-20",
          //   duration: 3,
          //   progress: 0.4,
          //   info: "lorem ipsum",
          // },
        ],
        links: [{ id: 1, source: 1, target: 2, type: "0" }],
      },
      selectedTask: null,
      messages: [],
    };
  },
  computed: {
    progressPercentage() {
      let taskProgress = this.selectedTask.progress;
      if (!taskProgress) {
        return "0";
      }
      return `${Math.round(+taskProgress * 100)} %`;
    },
    formattedStartDate() {
      let taskStart = this.selectedTask.start_date;
      return `${taskStart.getFullYear()} / ${
        taskStart.getMonth() + 1
      } / ${taskStart.getDate()}`;
    },
    formattedEndDate() {
      let taskEnd = this.selectedTask.end_date;
      return `${taskEnd.getFullYear()} / ${
        taskEnd.getMonth() + 1
      } / ${taskEnd.getDate()}`;
    },
  },
  methods: {
    selectTask(task) {
      this.selectedTask = task;
    },

    addMessage(message) {
      this.messages.unshift(message);
      if (this.messages.length > 40) {
        this.messages.pop();
      }
    },

    logTaskUpdate(id, mode, task) {
      let text = task && task.text ? ` (${task.text})` : "";
      let message = `Task ${mode}: ${id} ${text}`;
      this.addMessage(message);
    },

    logLinkUpdate(id, mode, link) {
      let message = `Link ${mode}: ${id}`;
      if (link) {
        message += ` ( source: ${link.source}, target: ${link.target} )`;
      }
      this.addMessage(message);
    },
  },
};
</script>

<style>
html,
body {
  height: 100%;
  margin: 0;
  padding: 0;
}

.container {
  height: 100vh;
  width: 100%;
}

.left-container {
  overflow: hidden;
  position: relative;
  height: 100%;
}

.right-container {
  border-right: 1px solid #cecece;
  float: right;
  height: 100%;
  width: 340px;
  box-shadow: 0 0 5px 2px #aaa;
  position: relative;
  z-index: 2;
}

.gantt-messages {
  list-style-type: none;
  height: 50%;
  margin: 0;
  overflow-x: hidden;
  overflow-y: auto;
  padding-left: 5px;
}

.gantt-messages > .gantt-message {
  background-color: #f4f4f4;
  box-shadow: inset 5px 0 #d69000;
  font-family: Geneva, Arial, Helvetica, sans-serif;
  font-size: 14px;
  margin: 5px 0;
  padding: 8px 0 8px 10px;
}

.gantt-selected-info {
  border-bottom: 1px solid #cecece;
  box-sizing: border-box;
  font-family: Geneva, Arial, Helvetica, sans-serif;
  height: 50%;
  line-height: 28px;
  padding: 10px;
}

.gantt-selected-info h2 {
  border-bottom: 1px solid #cecece;
}

.select-task-prompt h2 {
  color: #d9d9d9;
}
</style>
