<script>
  import { scaleLinear, scaleSqrt, scaleOrdinal } from "d3-scale";
  import { axisBottom, axisLeft } from "d3-axis";
  import { select } from "d3-selection";
  import { schemeCategory10 } from "d3-scale-chromatic";
  import { onMount } from "svelte";

  export let data;

  const [height, width] = [400, 600];
  const margin = { top: 50, right: 5, bottom: 55, left: 50 };
  const innerWidth = width - margin.left - margin.right;
  const innerHeight = height - margin.top - margin.bottom;

  const xScale = scaleLinear()
    .domain([0, d3.max(data, (d) => +d.income)])
    .range([0, innerWidth]);

  const yScale = scaleLinear()
    .domain([0, d3.max(data, (d) => +d.life_exp)])
    .range([innerHeight, 0]);

  const rScale = scaleSqrt()
    .domain([0, d3.max(data, (d) => +d.population)])
    .range([0, 25]);

  const colorScale = scaleOrdinal(schemeCategory10)
    .domain([...new Set(data.map((d) => d.continent))]);

  const xAxis = axisBottom(xScale);
  const yAxis = axisLeft(yScale);

  const title = "Gapminder Visualization (1800)";
  const xAxisLabel = "Income";
  const yAxisLabel = "Life Expectancy";
  const yearLabel = "1800";

  onMount(() => {
    const xAxisG = select("#x-axis").call(xAxis);
    const yAxisG = select("#y-axis").call(yAxis);
  });
</script>

<svg viewBox="0 0 {width} {height}" style="max-width: {width}px">
  <g transform="translate({margin.left}, {margin.top})">
    <g id="x-axis" transform="translate(0, {innerHeight})"></g>
    <g id="y-axis"></g>

    {#each data as d}
      <circle
        cx="{xScale(+d.income)}"
        cy="{yScale(+d.life_exp)}"
        r="{rScale(+d.population)}"
        fill="{colorScale(d.continent)}"
        opacity="0.8"
      ></circle>
    {/each}

    <text x="{innerWidth / 2}" y="{innerHeight + margin.bottom / 2}" text-anchor="middle" font-size="14">{xAxisLabel}</text>
    <text x="{-innerHeight / 2}" y="{-margin.left / 2}" text-anchor="middle" font-size="14" transform="rotate(-90)">{yAxisLabel}</text>

    <text x="{innerWidth / 2}" y="{-margin.top / 2}" text-anchor="middle" font-size="18" font-weight="bold">{title}</text>
    <text x="{innerWidth - margin.right}" y="{innerHeight - margin.bottom / 2}" text-anchor="end" font-size="14">{yearLabel}</text>
  </g>
</svg>
  