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
      endAngleScore: 0
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
    this.iterate();
    const circle = d3.select('#testCircle')
      .append('svg')
      .attr('width', this.width)
      .attr('height', this.height)
      .append('g')
      .attr('transform', "translate(" + this.width / 2  + "," + this.height / 2 + ")")

    circle.append('path')
      .attr('d', this.baseCircleGenerator)
      .attr('fill', '#e8eaed')

    let pBar =  d3.arc()
            .innerRadius(this.width / 2)
            .outerRadius(this.width / 2.5)
            .cornerRadius(this.cornerRadius)
            .startAngle(this.startAngle)
            .endAngle(this.endAngleCalculation)

    circle.append('path')
      .attr('id', 'animate')
      .attr('d', pBar)
      .attr('fill', this.fillColor)

    d3.select('#animate')
        .transition()
				.duration(1000)
        .attr('d', pBar.startAngle(0).endAngle(5))

    circle.append('text')
    .style('font-weight', 700)
    .style('text-anchor', 'middle')
    .style('alignment-baseline', 'central')
    .text(this.score)

    d3.select('text')
        .transition()
				.duration(1000)
        .attrTween('font-size', function(d){return d3.interpolate(3, 30)})
  },
  methods: {
    iterate: function () {
      let theScore = this.score
      if (theScore === 100) theScore++
      if (this.endAngleScore < theScore) {
        this.endAngleScore += 1
        setTimeout(this.iterate, 10)
      }
    }
  },
  computed: {
    arcGenerator () {
      return d3
        .arc()
        .innerRadius(this.width / 2)
        .outerRadius(this.width / 2.5)
        .cornerRadius(this.cornerRadius)
        .startAngle(this.startAngle)
        .endAngle(this.endAngleCalculation)
    },
    d () {
      return this.arcGenerator()
    },
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
