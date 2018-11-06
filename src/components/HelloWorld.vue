<template>
  <div class="hello">
    <h3>This is an element setup to test D3</h3>
    <div id='testCircle'></div>
  </div>
</template>

<script>

import * as d3 from 'd3'

export default {
  name: 'HelloWorld',
  props: {
    size: {
      type: Number,
      default: 100
    },
    score: {
      type: Number,
      default: 75
    }
  },
  mounted() {

    let circle = d3.select('#testCircle')
      .append('svg')
      .attr('width', this.size)
      .attr('height', this.size)

    let meter = circle.append('g')
      .attr('transform', "translate(" + this.size / 2  + "," + this.size / 2 + ")")

    let baseCircle = d3.arc()
      .innerRadius(this.size / 2)
      .outerRadius(this.size / 2.5)
      .cornerRadius(0)
      .startAngle(0)
      .endAngle(6.283)

    meter.append('path')
      .attr('d', baseCircle)
      .attr('fill', '#e8eaed')

    let arc =  d3.arc()
      .innerRadius(this.size / 2)
      .outerRadius(this.size / 2.5)
      .cornerRadius(50)
      .startAngle(0)

    let foreground = meter.append("path")
     .attr('id', 'animate')
     .attr('fill', this.fillColor)
     .datum({endAngle: 0})
     .attr("class", "background")
     .attr("d", arc);

    meter.append('text')
      .style('font-weight', 700)
      .style('text-anchor', 'middle')
      .style('alignment-baseline', 'central')
      .text(this.score)

    let textSize = this.size / 2.6

    d3.select('text')
      .transition()
			.duration(1000)
      .attrTween('font-size', function(d){return d3.interpolate(3, textSize)})

    this.updateUI(foreground, arc);

  },
  methods: {
    updateUI(foreground, arc){
      foreground.transition().duration(750).attrTween("d", this.arcTween(arc));
    },
    arcTween(arc) {
      return (d) => {
        let theEndAngle = this.endAngleCalculation
        if (this.endAngleCalculation === 6.283) theEndAngle = 7
        const interpolate = d3.interpolate(0, theEndAngle);
        return (t) => {
          d.endAngle = interpolate(t);
          return arc(d);
        };
      };
    }
  },
  computed: {
    endAngleCalculation () {
      return this.score / 100 * 6.283
    },
    fillColor () {
      if (this.score > 90) return '#028038'
      else if (this.score > 50) return '#ec7800'
      else return '#c7221f'
    }
  }
}
</script>

<style scoped>

</style>
