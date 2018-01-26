<template>
  <div class="cool-box">
    <div class="content">
      <div class="filters">
        <h4>Filter minimum skills</h4>
        <div class="range-box">
          <div>
            <span class="range-box__category">Viz: </span><span class="range-box__value">{{ minFilters.Viz }}</span>
          </div><input type="range" min="1" max="10" step="1" v-model="minFilters.Viz" @input="filterUpdate">
        </div>
        <div class="range-box">
          <div>
            <span class="range-box__category">Stats: </span><span class="range-box__value">{{ minFilters.Stats }}</span>
          </div><input type="range" min="1" max="10" step="1" v-model="minFilters.Stats" @input="filterUpdate">
        </div>
        <div class="range-box">
          <div>
            <span class="range-box__category">Maths: </span><span class="range-box__value">{{ minFilters.Maths }}</span>
          </div><input type="range" min="1" max="10" step="1" v-model="minFilters.Maths" @input="filterUpdate">
        </div>
        <div class="range-box">
          <div>
            <span class="range-box__category">Arts: </span><span class="range-box__value">{{ minFilters.Arts }}</span>
          </div><input type="range" min="1" max="10" step="1" v-model="minFilters.Arts" @input="filterUpdate">
        </div>
        <div class="range-box">
          <div>
            <span class="range-box__category">CompUse: </span><span class="range-box__value">{{ minFilters.CompUse }}</span>
          </div><input type="range" min="1" max="10" step="1" v-model="minFilters.CompUse" @input="filterUpdate">
        </div>
        <div class="range-box">
          <div>
            <span class="range-box__category">Prog: </span><span class="range-box__value">{{ minFilters.Prog }}</span>
          </div><input type="range" min="1" max="10" step="1" v-model="minFilters.Prog" @input="filterUpdate">
        </div>
        <div class="range-box">
          <div>
            <span class="range-box__category">GFXProg: </span><span class="range-box__value">{{ minFilters.GFXProg }}</span>
          </div><input type="range" min="1" max="10" step="1" v-model="minFilters.GFXProg" @input="filterUpdate">
        </div>
        <div class="range-box">
          <div>
            <span class="range-box__category">HCIProg: </span><span class="range-box__value">{{ minFilters.HCIProg }}</span>
          </div><input type="range" min="1" max="10" step="1" v-model="minFilters.HCIProg" @input="filterUpdate">
        </div>
        <div class="range-box">
          <div>
            <span class="range-box__category">UXEval: </span><span class="range-box__value">{{ minFilters.UXEval }}</span>
          </div><input type="range" min="1" max="10" step="1" v-model="minFilters.UXEval" @input="filterUpdate">
        </div>
        <div class="range-box">
          <div>
            <span class="range-box__category">Comm: </span><span class="range-box__value">{{ minFilters.Comm }}</span>
          </div><input type="range" min="1" max="10" step="1" v-model="minFilters.Comm" @input="filterUpdate">
        </div>
        <div class="range-box">
          <div>
            <span class="range-box__category">Collab: </span><span class="range-box__value">{{ minFilters.Collab }}</span>
          </div><input type="range" min="1" max="10" step="1" v-model="minFilters.Collab" @input="filterUpdate">
        </div>
        <div class="range-box">
          <div>
            <span class="range-box__category">Repo: </span><span class="range-box__value">{{ minFilters.Repo }}</span>
          </div><input type="range" min="1" max="10" step="1" v-model="minFilters.Repo" @input="filterUpdate">
        </div>
        <h4>Full-text filters</h4>
        <div class="text-box">
          <label>
            Hobbies
          </label><input type="text" v-model="textFilters.Hobbies" @input="filterUpdate">
        </div>
        <div class="text-box">
          <label>
            Learning
          </label><input type="text" v-model="textFilters.Learn" @input="filterUpdate">
        </div>
        <div class="text-box">
          <label>
            Degree
          </label><input type="text" v-model="textFilters.Degree" @input="filterUpdate">
        </div>
        <div class="text-box">
          <label>
            Major
          </label><input type="text" v-model="textFilters.Major" @input="filterUpdate">
        </div>
      </div>
      <svg class="cool-box__svg" width="500" height="500" style="1px solid black" ref="svg">
        <g class="x-axis-top" />
        <g class="x-axis-bottom" />
        <g class="y-axis-left" />
        <g class="y-axis-right" />
        <g class="lines" />
      </svg>
    </div>
    <table class="table">
      <tr>
        <th class="table__heading table__heading--alias">Alias</th>
        <th class="table__heading table__heading--hobbies">Hobbies</th>
        <th class="table__heading table__heading--learning">Learning goals</th>
        <th class="table__heading table__heading--degree">Degree</th>
        <th class="table__heading table__heading--major">Major</th>
      </tr>
      <tr class="table__row" v-for="datapoint in filteredData" tabindex="0" @focusin="focusIn(datapoint)" @focusout="focusOut(datapoint)" @mouseover="hoverIn(datapoint)" @mouseout="hoverOut(datapoint)">
        <td class="table__cell table__cell--alias"> {{ datapoint.Alias }} </td>
        <td class="table__cell table__cell--hobbies" > {{ datapoint[qualityKeys.Hobbies] }} </td>
        <td class="table__cell table__cell--learning" > {{ datapoint[qualityKeys.Learn] }} </td>
        <td class="table__cell" > {{ datapoint[qualityKeys.Degree] }} </td>
        <td class="table__cell table__cell--major" > {{ datapoint[qualityKeys.Major] }} </td>
      </tr>
    </table>
  </div>
</template>

<script>
import { select, csvParse, scaleLinear, scaleOrdinal, schemeCategory20, axisTop, axisRight, axisLeft, axisBottom, line, transition } from 'd3';
import data from '@/data.csv';

export default {
  computed: {
    filteredData() {
      return this.filteredPoints.map(i => this.coolData[i]);
    },
    filteredLines() {
      return this.filteredPoints.map(i => this.lineData[i]);
    },
  },
  created() {
    this.coolData = csvParse(data);

    // used for colors
    this.majors = this.coolData.map(point => point[this.qualityKeys.Major])
      .filter((major, i, self) => i === self.indexOf(major));
    this.majors.sort();

    this.filteredPoints = Array(this.coolData.length).fill().map((e, i) => (i));
    this.categories = [
      'Viz',
      'Stats',
      'Maths',
      'Arts',
      'CompUse',
      'Prog',
      'GFXProg',
      'HCIProg',
      'UXEval',
      'Comm',
      'Collab',
      'Repo',
    ];
    this.lineColor = scaleOrdinal(schemeCategory20);
    this.updateScale();
    this.lineData = this.rows2lines();
  },
  data() {
    return {
      height: 550,
      width: 800,
      xAxisTop: null,
      xAxisBottom: null,
      yAxisLeft: null,
      yAxisRight: null,
      lineColor: null,
      minFilters: {
        Viz: 1,
        Stats: 1,
        Maths: 1,
        Arts: 1,
        CompUse: 1,
        Prog: 1,
        GFXProg: 1,
        HCIProg: 1,
        UXEval: 1,
        Comm: 1,
        Collab: 1,
        Repo: 1,
      },
      textFilters: {
        Hobbies: '',
        Degree: '',
        Learn: '',
        Major: '',
      },
      coolData: [],
      lineData: [],
      filteredPoints: [],
      categories: [],
      majors: [],
      dataKeys: {
        Viz: 'How would you rate your Information Visualization skills?',
        Stats: 'How would you rate your statistical skills?',
        Maths: 'How would you rate your mathematics skills?',
        Arts: 'How would you rate your drawing and artistic skills?',
        CompUse: 'How would you rate your computer usage skills?',
        Prog: 'How would you rate your programming skills?',
        GFXProg: 'How would you rate your computer graphics programming skills?',
        HCIProg: 'How would you rate your human-computer interaction programming skills?',
        UXEval: 'How would you rate your user experience evaluation skills?',
        Comm: 'How would you rate your communication skills?',
        Collab: 'How would you rate your collaboration skills?',
        Repo: 'How would you rate your code repository skills?',
      },
      qualityKeys: {
        Hobbies: 'Please, tell me about yourself. What interest you? Do you have any hobbies?',
        Degree: 'What degree are you pursuing?',
        Learn: 'What do you expect to learn in Information Visualization? How do expect to use what you learn?',
        Major: 'What is your Major?',
      },
      dataKeysOrder: [
        'Viz',
        'Stats',
        'Maths',
        'Arts',
        'CompUse',
        'Prog',
        'GFXProg',
        'HCIProg',
        'UXEval',
        'Comm',
        'Collab',
        'Repo',
      ],
    };
  },
  methods: {
    // i is index in filteredPoints
    majorIndex(i) {
      const datapoint = this.coolData[this.filteredPoints[i]];
      return this.majors.indexOf(datapoint[this.qualityKeys.Major]);
    },
    updateScale() {
      this.yScale = scaleLinear()
        .domain([10, 1])
        .range([0, this.height]);
      this.xScale = scaleOrdinal()
        .domain(this.categories)
        .range(Array(this.categories.length).fill().map((e, i) => (
          i * (this.width / (this.categories.length - 1))
        )));
    },
    updateWidth() {
      this.width = this.$refs.svg.clientWidth - 60; // 60 is the padding on svg
      this.updateScale();
      this.updateAxes();

      this.createLines();
    },
    createLines() {
      const coolLine = line()
        .x(d => this.xScale(d.x))
        .y(d => this.yScale(d.y));

      const base = select(this.$refs.svg);

      base.select('g.lines')
        .selectAll('path')
        .data(this.filteredLines)
        .enter()
        .append('path');

      base.select('g.lines')
        .selectAll('path')
        .data(this.filteredLines)
        .exit()
        .remove();

      base.select('g.lines')
        .selectAll('path')
        .data(this.filteredLines)
        .classed('cool-line', true)
        .transition()
        .attr('d', coolLine)
        .attr('id', (_, i) => this.formatId(this.coolData[this.filteredPoints[i]].Alias))
        .attr('stroke', (_, i) => this.lineColor(this.majorIndex(i)));
    },
    rows2lines() {
      const outerRes = [];
      this.coolData.forEach((dataPoint) => {
        const innerRes = [];
        this.dataKeysOrder.forEach((key) => {
          innerRes.push({ x: key, y: dataPoint[this.dataKeys[key]] });
        });
        outerRes.push(innerRes);
      });

      return outerRes;
    },
    initAxes() {
      this.xAxisBottom = axisBottom(this.xScale);
      this.xAxisTop = axisTop(this.xScale);
      this.yAxisLeft = axisLeft(this.yScale);
      this.yAxisRight = axisRight(this.yScale);

      // x-axis
      select(this.$refs.svg).select('g.x-axis-top')
        .call(this.xAxisTop);
      select(this.$refs.svg).select('g.x-axis-bottom')
        .attr('transform', `translate(0, ${this.height})`)
        .call(this.xAxisBottom);

      select(this.$refs.svg).selectAll('g.x-axis-bottom g.tick')
        .append('line')
        .classed('grid-line', true)
        .attr('x1', 0)
        .attr('x2', 0)
        .attr('y1', 0)
        .attr('y2', -this.height);

      // y-axis
      select(this.$refs.svg).select('g.y-axis-left')
        .call(this.yAxisLeft);
      select(this.$refs.svg).select('g.y-axis-right')
        .attr('transform', `translate(${this.width})`)
        .call(this.yAxisRight);

      select(this.$refs.svg).selectAll('g.y-axis-left g.tick')
        .append('line')
        .classed('grid-line', true)
        .attr('x1', 0)
        .attr('x2', this.width)
        .attr('y1', 0)
        .attr('y2', 0);
    },
    updateAxes() {
      this.xAxisBottom.scale(this.xScale);
      this.xAxisTop.scale(this.xScale);
      this.yAxisLeft.scale(this.yScale);
      this.yAxisRight.scale(this.yScale);

      // x-axis
      select(this.$refs.svg).select('g.x-axis-top')
        .call(this.xAxisTop);
      select(this.$refs.svg).select('g.x-axis-bottom')
        .attr('transform', `translate(0, ${this.height})`)
        .call(this.xAxisBottom);

      select(this.$refs.svg).selectAll('g.x-axis-bottom g.tick line.grid-line')
        .attr('y2', -this.height);

      // y-axis
      select(this.$refs.svg).select('g.y-axis-left')
        .call(this.yAxisLeft);
      select(this.$refs.svg).select('g.y-axis-right')
        .attr('transform', `translate(${this.width})`)
        .call(this.yAxisRight);

      select(this.$refs.svg).selectAll('g.y-axis-left g.tick line.grid-line')
        .attr('x2', this.width);
    },
    focusIn(d) {
      select(this.$refs.svg).select(`#${this.formatId(d.Alias)}`)
        .classed('cool-line--focus', true);
    },
    focusOut(d) {
      select(this.$refs.svg).select(`#${this.formatId(d.Alias)}`)
        .classed('cool-line--focus', false);
    },
    hoverIn(d) {
      select(this.$refs.svg).select(`#${this.formatId(d.Alias)}`)
        .classed('cool-line--hover', true);
    },
    hoverOut(d) {
      select(this.$refs.svg).select(`#${this.formatId(d.Alias)}`)
        .classed('cool-line--hover', false);
    },
    formatId(str) {
      return str.trim().toLowerCase().replace(/ /g, '-');
    },
    filterUpdate() {
      // filter the minimum skills
      let tempFiltered = Array(this.coolData.length).fill().map((e, i) => (i));
      Object.keys(this.minFilters).forEach((category) => {
        const filterValue = this.minFilters[category];
        tempFiltered = tempFiltered.reduce((acc, point) => {
          const datapoint = this.coolData[point];
          if (parseInt(datapoint[this.dataKeys[category]], 10) >= filterValue) acc.push(point);
          return acc;
        }, []);
      });

      // filter the text
      Object.keys(this.textFilters).forEach((category) => {
        const filterValue = this.textFilters[category];
        tempFiltered = tempFiltered.reduce((acc, point) => {
          const datapoint = this.coolData[point];
          if (datapoint[this.qualityKeys[category]]
            .toLowerCase().includes(filterValue.toLowerCase())) acc.push(point);
          return acc;
        }, []);
      });

      this.filteredPoints = tempFiltered;
    },
  },
  mounted() {
    select(this.$refs.svg)
      .selectAll('circle')
      .attr('stroke', 'black');

    this.initAxes();
    this.updateWidth();

    this.$nextTick(() => {
      window.addEventListener('resize', () => {
        this.updateWidth();
      });
    });
  },
  name: 'cool-box',
  watch: {
    filteredPoints() {
      this.createLines();
    },
  },
};
</script>

<style lang="scss">
// wish I could use scoped with d3, 
// find a loader for it or something?

.content {
  display: flex;
}

.cool-box__svg {
  height: 550px;
  background: #f9f9f9;
  border: 1px solid #eee;
  padding: 30px;
  flex-grow: 1;
}

.filters {
  width: 250px;
  padding: 0 20px;

  h4 {
    border-bottom: 2px solid #333;
  }
}

.range-box {
  text-align: left;
  border-bottom: 1px solid #eee;
  padding: 2px 0;
  font-size: 12px;


  >div {
    display: inline-block;
    vertical-align: top;
    padding-top: 4px;
    width: 100px;
  }

  >input {
    display: inline-block;
    width: 150px;
    margin: 0;
  }
}

.range-box__category {
  font-weight: 500;
}

.range-box__value {
  color: #999;
}

.text-box {
  margin-bottom: 10px;
  text-align: left;
  font-size: 12px;


  >label {
    font-size: 12px;
    font-weight: 500;
    text-align: left;
    display: inline-block;
    width: 70px;
  }

  >input {
    padding: 5px;
    border-radius: 2px;
    border: 1px solid #999;
    box-sizing: border-box;
    width: 180px;
    font-size: 12px;
  }
}

.table { 
  table-layout:fixed;
  width:100%;
  border-top: 3px solid #333;
  margin-top: 20px;
  border-collapse: collapse;
}

.table__heading {
  font-weight: 500;
  border-bottom: 2px solid #333;
}

.table__heading--alias {
  width: 180px;
}

.table__heading--degree {
  width: 180px;
}

.table__heading--major {
  width: 180px;
}

.table__row {
  &:nth-child(even) {
    .table__cell {
      background: #fafafa;
    }
  }

  &:nth-child(odd) {
    .table__cell {
      background: #f3f3f3;
    }
  }

  &:hover {
    .table__cell {
      background: #FFB8B8;
    }
  }

  &:focus {
    outline: none;
    .table__cell {
      background: #FF9494;
      white-space: normal;
    }
  }
}

.table__cell {
  text-align: left;
  vertical-align: top;
  padding: 5px;
}

.table__cell--hobbies, .table__cell--learning, .table__cell--major {
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}

.cool-line {
  fill: none;
  stroke-width: 1;
}

.cool-line--focus {
  stroke-width: 10 !important;
}

.cool-line--hover {
  stroke-width: 5;
}

.grid-line {
  fill: #ccc;
  stroke: #ccc;
  stroke-width: 0.1;
}
</style>
