<template>
  <div class="container">
    <h2>SPRINKLER IRRIGATION MANAGEMENT APPLICATION</h2>

    <div class="right-container">
      <div class="gantt-selected-info">
        <div v-if="selectedTask">
          <h2>{{ selectedTask.text }}</h2>
          <!-- <span><b>ID: </b>{{ selectedTask.id }}</span> -->
          <!-- <span><b>Progress: </b> {{ progressPercentage }}</span -->
          <!-- <br /> -->
          <!-- <span><b>Start Date: </b>{{ formattedStartDate }}</span -->

          <!-- <span><b>End Date: </b>{{ formattedEndDate }}</span -->

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
            info: [
              {
                first:
                  "Land Clearing Equipment: Renting or hiring equipment such as bulldozers, excavators, or land clearing machinery. Cost: $1,500 to $3,000 per day (varies based on equipment type and size) Duration: Estimate 5-7 days for clearing a 5-hectare plot. Labor: Hiring laborers for tasks like removing trees, shrubs, and debris. Cost: $15 to $25 per hour per laborer Duration: Estimate 5 laborers working 8 hours per day for 7 days.",
              },
            ],
          },
          {
            id: 9,
            text: "Pipework Installation:",
            start_date: "2023-06-20",
            duration: 3,
            progress: 0.4,
            info: [
              {
                first:
                  "Pipe Material: PVC Pipes: Commonly used for irrigation systems. Cost: Approximately $0.50 to $2 per linear foot, depending on diameter and quality. Example: For a 5-acre field (approximately 21,780 square feet), with pipes spaced 10 feet apart, you might need about 2,178 linear feet of PVC pipes.",
                second:
                  "Labor: Pipe Installation Labor: Hiring skilled labor for digging trenches, laying pipes, and connecting fittings. Cost: Approximately $20 to $40 per hour per laborer. Duration: The time required depends on the complexity of the installation but could range from several days to a few weeks for a larger area.",
                third:
                  "Total Estimated Cost for Pipework Installation: Pipe Material: Varies based on pipe length needed. Labor Costs: Varies based on labor rates and project duration. Fittings and Accessories: Approximately 10% to 20% of the pipe cost. Miscellaneous Costs: Equipment rental and backfill material costs if applicable.",
              },
            ],
          },
          {
            id: 10,
            text: "Sprinkler Installation:",
            start_date: "2023-06-22",
            duration: 3,
            progress: 0.4,
            info: [
              {
                first: `Sprinkler System Components: Sprinkler Heads: The cost of the sprinkler heads depends on their type, quality, and coverage area. Common types include rotor and spray heads. Cost: Approximately $2 to $20 per sprinkler head. Pipes and Fittings: Costs for the mainline and lateral pipes, as well as fittings like couplers, tees, elbows, and risers.

Cost: Varies based on pipe diameter and length but may range from $0.50 to $3 per linear foot.
Valves and Controllers: Costs for control valves and irrigation controllers.

Cost: Varies based on the number of zones and system complexity but may range from $50 to $500 per zone.
Backflow Prevention Device: If required by local regulations, this device prevents contamination of the water supply.

Cost: Typically $50 to $300.`,
                second: "",
                third: "",
              },
            ],
          },
          {
            id: 11,
            text: "Valve and Control Installation:",
            start_date: "2023-06-23",
            duration: 3,
            progress: 0.4,
            info: [
              {
                first: `The cost of valve and control installation for an irrigation system can vary depending on factors such as the system's complexity, the number of zones, and local labor rates. Here's an example of estimated values for valve and control installation:

Valve and Control Installation
Control Valves:
Cost per Valve: The cost of control valves varies based on the type and quality of the valves.
Cost: Approximately $50 to $150 per control valve.
Example: If you have 5 irrigation zones and each zone requires a control valve, the cost would range from $250 to $750 for control valves.
Irrigation Controller:
Controller Type: The cost of the irrigation controller depends on its features and capabilities.
Cost: Approximately $100 to $400 for a basic controller, and up to $1,000 or more for advanced smart controllers.
Electrical Wiring and Conduit:
Wiring: Costs for electrical wiring and conduit to connect the controller to the control valves.
Cost: Variable based on the distance between the controller and valves and the complexity of the wiring.
Labor:
Installation Labor: Hiring skilled labor for installing control valves, the irrigation controller, and electrical wiring.
Cost: Approximately $40 to $70 per laborer per hour.
Duration: Installation can typically be completed within a day for a standard residential system but may take longer for more complex commercial systems.`,
              },
            ],
          },
          {
            id: 12,
            text: "Pump Installation:",
            start_date: "2023-06-24",
            duration: 3,
            progress: 0.4,
            info: [
              {
                first: `Pump Installation
Pump Selection:
Pump Type: The cost of the pump depends on whether it's a submersible pump or a surface pump. Submersible pumps are used for deep wells, while surface pumps are typically used for shallow water sources.

Cost: Submersible pumps can range from $500 to $2,000 or more, depending on capacity and quality. Surface pumps can vary from $200 to $800.
Pump Capacity: The size and capacity of the pump are determined by the water demand and well depth (if applicable).

Cost: The cost increases with higher capacity pumps.
Well Drilling (if applicable):
Well Drilling: If drilling a well is necessary, the cost can vary greatly based on the well depth, geological conditions, and location.
Cost: Well drilling can range from $1,500 for shallow wells to $10,000 or more for deep wells.`,
              },
            ],
          },
          {
            id: 13,
            text: "System Testing:",
            start_date: "2023-06-20",
            duration: 3,
            progress: 0.4,
            info: [
              {
                first: `Professional Testing:
Professional Services: Hiring an irrigation specialist or technician to conduct a comprehensive system test.

Cost: Professional testing services can range from $200 to $500 or more, depending on the complexity of the system and the size of the area.
Travel and Expenses: If the specialist needs to travel to your location, you may need to cover travel expenses, including mileage or accommodation costs.`,
                second: `DIY Testing:
If you choose to conduct system testing yourself, you may need to invest in some testing equipment and tools. Here are some example costs:

Pressure Gauge: To measure water pressure at different points in the system.

Cost: $20 to $50.
Flow Meter: To measure the flow rate of water through the system.

Cost: $50 to $150.
Rain Gauge: For measuring rainfall and adjusting irrigation schedules.

Cost: $10 to $30.
Soil Moisture Sensors: Optional for monitoring soil moisture levels.

Cost: $20 to $100 per sensor, depending on type and quality.
Multimeter: For checking electrical connections (if applicable).

Cost: $20 to $50.`,
              },
            ],
          },
          {
            id: 14,
            text: "Adjustments:",
            start_date: "2023-06-19",
            duration: 3,
            progress: 0.4,
            info: [
              {
                first: `Replacement Parts: If adjustments require replacing components such as sprinkler heads, valves, or pipes.

Cost: Variable, depending on the parts needed.
Tools: Tools for making adjustments, such as wrenches, pliers, screwdrivers, and pipe cutters.

Cost: Variable, depending on the tools you already have and those you need to purchase.`,
              },
            ],
          },
          {
            id: 15,
            text: "Safety Checks:",
            start_date: "2023-06-18",
            duration: 3,
            progress: 0.4,
            info: [
              {
                first: `Safety Gear: Safety equipment such as gloves, safety glasses, and hearing protection.

Cost: Variable, depending on the gear you need to purchase.
Tools: Tools for inspecting the system, such as a multimeter for checking electrical connections (if applicable).

Cost: Variable, depending on the tools you already have and those you need to purchase.`,
              },
            ],
          },
          {
            id: 16,
            text: "Maintenance:",
            start_date: "2023-06-16",
            duration: 3,
            progress: 0.4,
            info: [
              {
                first: `If you choose to perform maintenance on your irrigation system yourself, you may need to invest in tools and materials. Here are some example costs:

Replacement Parts: The cost of replacement parts, such as sprinkler heads, valves, or pipes, if any components need to be replaced during maintenance.

Cost: Variable, depending on the parts needed.
Tools: Tools for maintenance tasks, such as wrenches, pliers, screwdrivers, and pipe cutters.

Cost: Variable, depending on the tools you already have and those you need to purchase.`,
                second: `Miscellaneous Costs:
Lubricants and Cleaning Supplies: Costs for lubricants, cleaning agents, and other supplies needed for maintenance tasks.

Cost: Variable, depending on the specific products used.
Documentation: Keeping records of maintenance activities, adjustments, and any required repairs is advisable, which may require notebooks or software tools.

Cost: Minimal`,
              },
            ],
          },

          {
            id: 17,
            text: "Monitoring and Optimization:",
            start_date: "2023-06-17",
            duration: 3,
            progress: 0.4,
            info: [
              {
                first: `Weather Stations: Weather monitoring equipment, such as temperature sensors, rainfall gauges, and wind sensors.

Cost: Variable, depending on the specific equipment you choose.
Soil Moisture Sensors: Sensors for monitoring soil moisture levels at different depths in the root zone.

Cost: $20 to $100 per sensor, depending on type and quality.
Irrigation Scheduling Software: If you use software tools for optimization, there may be subscription or licensing fees.

Cost: Variable, depending on the software.`,
                second: `Data Logging and Storage: Costs for data logging equipment or cloud storage services to store monitoring data.

Cost: Variable, depending on the data storage needs.
Documentation: Keeping records of monitoring results, adjustments, and optimization activities is advisable, which may require notebooks or software tools.

Cost: Minimal.`,
              },
            ],
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
