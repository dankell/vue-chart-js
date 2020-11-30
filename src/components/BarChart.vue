<script>
import { Bar, mixins } from 'vue-chartjs'
import 'chartjs-plugin-annotation'

const { reactiveProp } = mixins

export default {
  extends: Bar,
  mixins: [reactiveProp],
  props: ['chartData', 'options'],
  mounted () {
    this.renderChart(this.chartData, this.options)
  }
}
</script>
