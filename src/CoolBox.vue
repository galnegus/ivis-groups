<template>
  <div class="cool-box">
    <button @click="removeBar">REMOVE</button>
    <svg class="cool-box__svg" width="500" height="500" style="1px solid black" ref="svg">
      <g class="x-axis" />
      <g class="y-axis" />
    </svg>
  </div>
</template>

<script>
import { select, csvParse, scaleLinear, axisLeft, axisBottom, transition } from 'd3';
import data from '@/data.csv';

export default {
  created() {
    const coolData = csvParse(data);
    this.yScale = scaleLinear()
      .domain([0, 100])
      .range([0, 500]);
    this.xScale = scaleLinear()
      .domain([0, 5])
      .range([0, 500]);
  },
  data() {
    return {
      circles: [
        100,
        200,
        300,
        400,
        500,
      ],
      datapoints: [
        25,
        50,
        84,
        14,
        96,
      ],
    };
  },
  methods: {
    removeBar() {
      this.datapoints.splice(-1, 1);
      this.createBars();
      this.updateAxes();
    },
    removeCircle() {
      this.circles.splice(-1, 1);
      this.createCircles();
    },
    createBars() {
      const base = select(this.$refs.svg);
      const width = 500 / this.datapoints.length;

      base.selectAll('rect')
        .data(this.datapoints)
        .enter()
        .append('rect');

      base.selectAll('rect')
        .data(this.datapoints)
        .exit()
        .remove();

      base.selectAll('rect')
        .data(this.datapoints)
        .classed('cool-rect', true)
        .transition()
        .attr('x', (_, i) => i * width)
        .transition()
        .attr('y', d => 500 - this.yScale(d))
        .transition()
        .attr('width', 500 / this.datapoints.length)
        .transition()
        .attr('height', d => this.yScale(d));
    },
    createCircles() {
      const base = select(this.$refs.svg);

      base.selectAll('circle')
        .data(this.circles)
        .enter()
        .append('circle');

      base.selectAll('circle')
        .data(this.circles)
        .exit()
        .remove();

      base.selectAll('circle')
        .data(this.circles)
        .attr('cy', d => this.scale(d))
        .attr('cx', this.scale(40))
        .attr('r', this.scale(30))
        .attr('fill', 'red');
    },
    updateAxes() {
      const xAxis = axisBottom(this.xScale)
        .ticks(this.datapoints.length);

      const yAxis = axisLeft(this.yScale)
        .ticks(this.datapoints.length);

      // x-axis
      select(this.$refs.svg).select('g.x-axis')
        .attr('transform', 'translate(0, 500)')
        .call(xAxis);

      select(this.$refs.svg).selectAll('g.x-axis g.tick')
        .append('line')
        .classed('grid-line', true)
        .attr('x1', 0)
        .attr('x2', 0)
        .attr('y1', 0)
        .attr('y2', -500);

      // y-axis
      select(this.$refs.svg).select('g.y-axis')
        .call(yAxis);

      select(this.$refs.svg).selectAll('g.y-axis g.tick')
        .append('line')
        .classed('grid-line', true)
        .attr('x1', 0)
        .attr('x2', 500)
        .attr('y1', 0)
        .attr('y2', 0);
    },
  },
  mounted() {
    select(this.$refs.svg)
      .selectAll('circle')
      .attr('stroke', 'black');

    this.updateAxes();
    this.createBars();
  },
  name: 'cool-box',
};
</script>

<style lang="scss">
// wish I could use scoped with d3, 
// find a loader for it or something?

.cool-box__svg {
  width: 500px;
  height: 500px;
  background: #eee;
  border: 1px solid #ccc;
  padding: 40px;
}

.cool-rect {
  fill: #ddd;
  fill-opacity: 0.4;
  stroke: #bbb;
  stroke-width: 1;
}

.grid-line {
  fill: #ccc;
  stroke: #ccc;
  stroke-width: 0.3;
}
</style>
