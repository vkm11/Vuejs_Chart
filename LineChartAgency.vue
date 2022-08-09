<script>
  import { Line } from 'vue-chartjs'
  export default {
    extends: Line,
    props:['chart'],
    watch: {
        chart () {
            console.log(this.chart);
            this.$data._chart.update(); 
        },
    },  
    data () {
      return {
        chartData:{
          labels: this.chart.date,
          datasets: [ 
            {
              label: 'Agency',
              data: this.chart.agencies,
              fill: false,
              borderColor: '#2554FF',
              backgroundColor: '#2554FF',
              borderWidth: 1
            }
          ]
        },
        options: {
          scales: {
            yAxes: [{
              ticks: {
                beginAtZero: true
              },
              gridLines: {
                display: true
              }
            }],
            xAxes: [ {
              gridLines: {
                display: false
              }
            }]
          },
          legend: {
            display: true
          },
          responsive: true,
          maintainAspectRatio: false
        }
      }
    },
    mounted () {
      this.renderChart(this.chartData, this.options)
    }
  }
</script>
