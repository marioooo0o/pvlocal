<template>
  <div class="chart-container" style="text-align: center">
  <!-- <div class="chart-box" style="position: relative; width: 60vw;"> -->
    <canvas id="bar-chart" v-show="!loading" ></canvas>
  </div>
</template>

<script>
import Chart from "chart.js";

export default {
  name: "BarChart",
  inject: ['getCalculatedData'],
  data() {
    return {
      loading: true,
       labels: [],
      recover: [],
      active: [],
      reactive: [],
      lineChart: {
        type: "bar",
        data: {
          labels: [],
          datasets: [
            {
              label: "Pobrana",
              fill: false,
              data: [],
              backgroundColor: "rgba(201, 0, 0,.5)",
              borderColor: "#c90000",
              borderWidth: 1.5,
              lineTension: 0.15,
            },
            {
              label: "Oddana",
              fill: false,
              data: [],
              backgroundColor: "rgba(0, 0, 0,.5)",
              borderColor: "#292828",
              borderWidth: 1.5,
              lineTension: 0.15,
            },
            {
              label: "Do odzyskania",
              fill: false,
              data: [],
              backgroundColor: "rgba(33, 183, 132,.5)",
              borderColor: "#21b784",
              borderWidth: 1.5,
              lineTension: 0.15,
            },
          ],
        },
        options: {
          responsive: true,
          legend: {
            labels: {
              fontSize: 16,
            },
          },
          lineTension: 1,
          scales: {
            xAxes: [
              {
                scaleLabel: {
                  display: true,
                  labelString: "Data odczytu",
                  fontSize: 20,
                },
              },
            ],
            yAxes: [
              {
                scaleLabel: {
                  display: true,
                  labelString: "Energia [kWh]",
                  fontSize: 20,
                },
                ticks: {
                  beginAtZero: true,
                  padding: 15,
                  stepSize: 200,
                },
              },
            ],
          },
        },
      },
    };
  },
  computed:{
    calculatedData(){
      return this.getCalculatedData();
    }
  },
  created() {
    this.splitIntoParts(this.calculatedData, this.labels, this.active, this.reactive, 'month', this.recover);
  },
  beforeMount() {
    this.getDataToChart();
  },
  mounted() {
    const ctx = document.getElementById("bar-chart");
    new Chart(ctx, this.lineChart);
  },
  methods: {
    splitIntoParts(readings, labels, active, reactive, typeLabel='date', recover = null) {
      if (recover) {
        if(typeLabel == 'date'){
          for (let reading in readings) {
          labels.push(readings[reading].date);
          active.push(readings[reading].activeEnergyConsumed);
          reactive.push(readings[reading].reactiveEnergyConsumed);
          recover.push(readings[reading].energyToRecover);
        }
        }
        else{
          for (let reading in readings) {
          labels.push(readings[reading].month);
          active.push(readings[reading].activeEnergyConsumed);
          reactive.push(readings[reading].reactiveEnergyConsumed);
           recover.push(readings[reading].energyToRecover);
        }
        }
      }
      else{
        if(typeLabel === 'date'){
          for (let reading in readings) {
          labels.push(readings[reading].date);
          active.push(readings[reading].activeEnergyConsumed);
          reactive.push(readings[reading].reactiveEnergyConsumed);
          
        }
        }
        else{
          for (let reading in readings) {
          labels.push(readings[reading].month);
          active.push(readings[reading].activeEnergyConsumed);
          reactive.push(readings[reading].reactiveEnergyConsumed);
         
        }
        }
      }
    },
    getDataToChart() {
      this.lineChart.data.labels = this.labels;
      this.lineChart.data.datasets[0].data = this.active;
      this.lineChart.data.datasets[1].data = this.reactive;
      this.lineChart.data.datasets[2].data = this.recover;
      
      this.loading = false;
    },
  },
};
</script>

<style scoped>
p {
  background-color: #292828;
  /* color: rgb(33, 183, 132)); */
}
.chart-box{
  text-align: center;
  position: relative;
}
@media screen and (max-width: 700px) { 
     .chart-box
    {
       width: 400px !important; 
    } 
}

@media screen and (max-width: 500px) { 
     .chart-box
    {
       width: 300px !important; 
    } 
}
</style>
