<script>
  import { onMount } from 'svelte';
  import { scaleBand, scaleLinear } from 'd3-scale';
  import { axisBottom, axisLeft } from 'd3-axis';
  import { select } from 'd3-selection';

  // Dimensions
  const width = 600;
  const height = 300;
  const margin = { top: 10, right: 10, bottom: 50, left: 60 };
  const innerWidth = width - margin.left - margin.right;
  const innerHeight = height - margin.top - margin.bottom;

  // Array
  const data = [
    { service: 'Netflix', viewers: 2.9 },
    { service: 'Amazon Prime Video', viewers: 1.3 },
    { service: 'Disney+', viewers: 2.1 },
    { service: 'Hulu', viewers: 0.9 },
    { service: 'Apple TV', viewers: 1.1 },
    { service: 'Rakuten', viewers: 0.4 },
  ];

  // Configure x- and y-scales
  const xScale = scaleBand()
    .domain(data.map(d => d.service))
    .range([0, innerWidth])
    .padding(0.1);

  const yScale = scaleLinear()
    .domain([0, Math.max(...data.map(d => d.viewers))])
    .range([innerHeight, 0]);

  // Create axes
  const xAxis = axisBottom(xScale);
  const yAxis = axisLeft(yScale);

  // Append axes to the chart
  let xAxisGroup;
  let yAxisGroup;

  function addAxes() {
    xAxisGroup = select('.x-axis').call(xAxis);
    yAxisGroup = select('.y-axis').call(yAxis);
  }

  onMount(addAxes);
</script>

<svg viewBox="0 0 {width} {height}" style="max-width: {width}px">
  <g transform={`translate(${margin.left},${margin.top})`}>
    {#each data as d}
      <rect
        x="{xScale(d.service)}"
        y="{yScale(d.viewers)}"
        width="{xScale.bandwidth()}"
        height="{innerHeight - yScale(d.viewers)}"
        fill="steelblue"
      />
    {/each}
    <g transform={`translate(0, ${innerHeight})`} class="x-axis" />
    <g class="y-axis" />
    <text
      transform="rotate(-90)"
      y="-{margin.left}"
      x="-{innerHeight / 2}"
      dy="1em"
      text-anchor="middle"
      fill="black"
      font-size="12px"
    >
      Viewers (millions)
    </text>
    <text
      x="{innerWidth / 2}"
      y="{innerHeight + margin.bottom}"
      text-anchor="middle"
      fill="black"
      font-size="12px"
    >
      Streaming Services
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
