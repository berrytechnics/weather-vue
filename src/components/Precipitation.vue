<template>
    <div class="main">
        <div class="header"><h1 class="text-center">Precipitation</h1></div>
        <Bar
        class="chart"
      :chart-data="chartData"
    />
    </div>
  </template>
  <script>
  import { Bar } from 'vue-chartjs'
  import { Chart as ChartJS, Title, Tooltip, Legend, BarElement, CategoryScale, LinearScale } from 'chart.js'
  ChartJS.register(Title, Tooltip, Legend, BarElement, CategoryScale, LinearScale)
  const labels = ()=>{
    let result = []
    for(let i=0; i<60; i++) {result.push(i)}
    return result
  }
  export default {
    name: 'Precipitation',
    components: { Bar },
    props: {
        weather:Object
    },
    data() {
      return {
        chartData: {
          labels: labels(),
          
          datasets:[
            {
                label:"% (next 60 mins.)",
                data:this.weather.data,
                backgroundColor:'#b2ca59'
            }
        ]
        },
        chartOptions: {
          responsive: true,
          maintainAspectRatio:false
        }
      }
    },
}
</script>
<style scoped>
</style>