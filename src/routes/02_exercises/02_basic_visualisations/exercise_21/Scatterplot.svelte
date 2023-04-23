<script>
  import { scaleLinear, scaleSqrt, scaleOrdinal } from 'd3-scale';
  import { axisBottom, axisLeft } from 'd3-axis';
  import { schemeCategory10 } from 'd3-scale-chromatic';
  import { select } from 'd3-selection';

  export let data = [];

  // Dimensions
  const [height, width] = [400, 600];
  const margin = { top: 50, right: 5, bottom: 55, left: 50 };
  const innerWidth = width - margin.left - margin.right;
  const innerHeight = height - margin.top - margin.bottom;

  // Scales
  const xScale = scaleLinear()
    .domain([0, d3.max(data, d => +d.income)])
    .range([0, innerWidth]);

  const yScale = scaleLinear()
    .domain([25, d3.max(data, d => +d.life_exp)])
    .range([innerHeight, 0]);

  const rScale = scaleSqrt()
    .domain([0, d3.max(data, d => +d.population)])
    .range([0, 20]);

  const colorScale = scaleOrdinal(schemeCategory10)
    .domain(data.map(d => d.continent));

  // Axes
  const xAxis = axisBottom(xScale);
  const yAxis = axisLeft(yScale);

  let xAxisGroup;
  let yAxisGroup;

  function addAxes() {
    xAxisGroup = select('.x-axis').call(xAxis);
    yAxisGroup = select('.y-axis').call(yAxis);
  }

  $: data, addAxes();
</script>

<svg viewBox="0 0 {width} {height}" style="max-width: {width}px">
  <g transform="translate({margin.left}, {margin.top})">
    {#each data as d}
      <circle
        cx="{xScale(+d.income)}"
        cy="{yScale(+d.life_exp)}"
        r="{rScale(+d.population)}"
        fill="{colorScale(d.continent)}"
      />
    {/each}
    <g transform={`translate(0, ${innerHeight})`} class="x-axis" />
    <g class="y-axis" />
    <text
      x="{innerWidth / 2}"
      y="{innerHeight + margin.bottom / 2}"
      text-anchor="middle"
      fill="black"
      font-size="12px"
    >
      Income
    </text>
    <text
      transform="rotate(-90)"
      y="-{margin.left}"
      x="-{innerHeight / 2}"
      dy="1em"
      text-anchor="middle"
      fill="black"
      font-size="12px"
    >
      Life Expectancy
    </text>
    <text x="{innerWidth / 2}" y="-{margin.top / 2}" text-anchor="middle" font-size="16px">
      Gapminder Visualization (1800)
    </text>
  </g>
</svg>

<style>
  .axis path,
  .axis line {
    fill: none;
    stroke: black;
    shape-rendering: crispEdges;
  }

  .axis text {
    font-family: sans-serif;
    font-size: 10px;
  }
</style>

  