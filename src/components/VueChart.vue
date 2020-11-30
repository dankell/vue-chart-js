<template>
  <div class="small">
    <button @click="fillData()">
      Randomize
    </button>
    <bar-chart
      v-if="loaded"
      :chart-data="chartData"
      :options="chartOptions"
    />
  </div>
</template>

<script>
import BarChart from './BarChart.vue'

export default {
  name: 'VueChart',
  components: {
    BarChart
  },
  data () {
    return {
      loaded: false,
      seed: 42,
      weekColor: ['red', 'orange', 'green'],
      chartColors: {
        red: 'rgb(255, 99, 132)',
        orange: 'rgb(255, 159, 64)',
        yellow: 'rgb(255, 205, 86)',
        green: 'rgb(75, 192, 192)',
        blue: 'rgb(54, 162, 235)',
        purple: 'rgb(153, 102, 255)',
        grey: 'rgb(201, 203, 207)'
      },
      chartOptions: {
        responsive: true,
        tooltips: {
          mode: 'index',
          intersect: true
        },
        scales: {
          xAxes: [
            {
              id: 'time-axis',
              type: 'time',
              stacked: true,
              offset: true,
              time: {
                unit: 'week',
                tooltipFormat: 'll',
                unitStepSize: 1
              },
              scaleLabel: {
                display: true,
                labelString: 'Date'
              }
            }
          ],
          yAxes: [
            {
              stacked: true
            }
          ]
        },
        annotation: {
          annotations: [
            {
              drawTime: 'afterDatasetsDraw',
              id: 'hline',
              type: 'line',
              mode: 'vertical',
              scaleID: 'time-axis',
              value: '2020-09-08 12:00',
              borderColor: 'black',
              borderWidth: 5,
              label: {
                backgroundColor: 'red',
                content: 'Test Label',
                enabled: true
              }
            }
          ]
        }
      },
      chartData: null
    }
  },
  mounted () {
    this.fillData()
  },
  methods: {
    rand (min, max) {
      const seed = this.seed
      min = min === undefined ? 0 : min
      max = max === undefined ? 1 : max
      this.seed = (seed * 9301 + 49297) % 233280
      return min + (this.seed / 233280) * (max - min)
    },
    randomScalingFactor () {
      return Math.round(this.rand(-100, 100))
    },
    async fillData () {
      this.loaded = false
      const weeks = this.setData()
      this.chartData = {
        datasets: []
      }
      for (let i = 0; i < weeks.length; i++) {
        const lineColor = this.chartColors[this.weekColor[i]]
        const series = {
          label: weeks[i].key,
          fill: true,
          borderColor: lineColor,
          backgroundColor: lineColor,
          borderWidth: 1.5,
          data: weeks[i].values,
          hidden: false
        }
        this.chartData.datasets.push(series)
      }
      // await this.setAnnotations()
      this.loaded = true
    },
    setData () {
      return [
        {
          key: 'Dataset 1',
          values: [
            {
              x: new Date('2020-08-22'),
              y: this.randomScalingFactor()
            },
            {
              x: new Date('2020-08-29'),
              y: this.randomScalingFactor()
            },
            {
              x: new Date('2020-09-05'),
              y: this.randomScalingFactor()
            },
            {
              x: new Date('2020-09-12'),
              y: this.randomScalingFactor()
            },
            {
              x: new Date('2020-09-19'),
              y: this.randomScalingFactor()
            },
            {
              x: new Date('2020-09-26'),
              y: this.randomScalingFactor()
            }
          ]
        },
        {
          key: 'Dataset 2',
          values: [
            {
              x: new Date('2020-08-22'),
              y: this.randomScalingFactor()
            },
            {
              x: new Date('2020-08-29'),
              y: this.randomScalingFactor()
            },
            {
              x: new Date('2020-09-05'),
              y: this.randomScalingFactor()
            },
            {
              x: new Date('2020-09-12'),
              y: this.randomScalingFactor()
            },
            {
              x: new Date('2020-09-19'),
              y: this.randomScalingFactor()
            },
            {
              x: new Date('2020-09-26'),
              y: this.randomScalingFactor()
            }
          ]
        },
        {
          key: 'Dataset 3',
          values: [
            {
              x: new Date('2020-08-22'),
              y: this.randomScalingFactor()
            },
            {
              x: new Date('2020-08-29'),
              y: this.randomScalingFactor()
            },
            {
              x: new Date('2020-09-05'),
              y: this.randomScalingFactor()
            },
            {
              x: new Date('2020-09-12'),
              y: this.randomScalingFactor()
            },
            {
              x: new Date('2020-09-19'),
              y: this.randomScalingFactor()
            },
            {
              x: new Date('2020-09-26'),
              y: this.randomScalingFactor()
            }
          ]
        }
      ]
    }
  }
}
</script>

<style>
.small {
  max-width: 600px;
  margin: 150px auto;
}
</style>
