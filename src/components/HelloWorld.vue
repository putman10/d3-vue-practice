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
  data () {
    return {
      endAngleScore: 4
    }
  },
  props: {
    innerRadius: {
      type: Number,
      default: 21.5
    },
    outerRadius: {
      type: Number,
      default: 25
    },
    cornerRadius: {
      type: Number,
      default: 50
    },
    startAngle: {
      type: Number,
      default: 0
    },
    width: {
      type: Number,
      default: 100
    },
    height: {
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
      .attr('width', this.width)
      .attr('height', this.height)

    let meter = circle.append('g')
      .attr('transform', "translate(" + this.width / 2  + "," + this.height / 2 + ")")

    let arc =  d3.arc()
      .innerRadius(this.width / 2)
      .outerRadius(this.width / 2.5)
      .cornerRadius(this.cornerRadius)
      .startAngle(this.startAngle)

    meter.append('path')
      .attr('d', this.baseCircleGenerator)
      .attr('fill', '#e8eaed')

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

    d3.select('text')
      .transition()
			.duration(1000)
      .attrTween('font-size', function(d){return d3.interpolate(3, 30)})

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
    baseCircleGenerator () {
      return d3
        .arc()
        .innerRadius(this.width / 2)
        .outerRadius(this.width / 2.5)
        .cornerRadius(0)
        .startAngle(0)
        .endAngle(6.283)
    },
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
