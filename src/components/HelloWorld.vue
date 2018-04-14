<template>
  <div id="chart" class="timeline">
  </div>
</template>

<script>
import * as d3 from 'd3'
import {timelines} from 'd3-timelines'
import chartData from './data'

export default {
  name: 'HelloWorld',
  data () {
    return {
      chart: null
    }
  },
  mounted () {
    let chart = timelines()
      .stack()
      .margin({left: 70, right: 20, top: 0, bottom: 0})
      .labelMargin(3)
      .itemHeight(7).itemMargin(3)
      .background('#eee')
      .tickFormat({
        format: d3.timeFormat('%Y.%m.%d'),
        tickTime: d3.timeDays,
        tickInterval: 1,
        tickSize: 6
      })
      .hover(function (d, i, datum) {
      // // d is the current rendering object
      // // i is the index during d3 rendering
      // // datum is the id object
      //   var div = $('#hoverRes');
      //   var colors = chart.colors();
      //   div.find('.coloredDiv').css('background-color', colors(i))
      //   div.find('#name').text(datum.label);
      })
      .click(function (d, i, datum) {
        // alert(datum.label)
      })
      .scroll((x, scale) => {
        // $("#scrolled_date").text(scale.invert(x) + " to " + scale.invert(x+width));
      })
    chart.axisZoom = false
    let svg = d3.select('#chart').append('svg')
    svg.attr('width', '1024px').datum(chartData).call(chart)
    let resize = () => {
      let bbox = svg.node().getBBox()
      svg.attr('height', bbox.y + bbox.height + 'px')
    }
    window.addEventListener('resize', resize)
    resize()
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.timeline {
  font-size: 8pt;
}
</style>
