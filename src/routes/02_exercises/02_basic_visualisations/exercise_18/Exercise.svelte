<script>
  import { onMount } from 'svelte';
  import { scaleLinear } from 'd3-scale';
  import { axisBottom } from 'd3-axis';
  import { select } from 'd3-selection';

  // Dimensions
  const width = 800;
  const height = 100;
  const margin = { top: 5, right: 5, bottom: 40, left: 5 };
  const innerWidth = width - margin.left - margin.right;
  const innerHeight = height - margin.top - margin.bottom;

  // Array
  const values = [2, 4, 6, 7, 9];

  // Linear scale
  const scale = scaleLinear().domain([1, 10]).range([0, innerWidth]);

  // Axis
  const xAxis = axisBottom(scale);

  onMount(() => {
    select('.x-axis')
      .call(xAxis)
      .select('.domain')
      .attr('stroke', 'currentColor');
  });
</script>

<svg viewBox="0 0 {width} {height}">
  <g transform="translate({margin.left},{margin.top})">
    {#each values as value}
      <circle
        cx="{scale(value)}"
        cy="{innerHeight / 2}"
        r="10"
        fill="teal"
      />
    {/each}
  </g>
  <g
    class="x-axis"
    transform="translate({margin.left},{height - margin.bottom})"
  >
    <text
      x="{innerWidth / 2}"
      y="{margin.bottom - 10}"
      text-anchor="middle"
      fill="currentColor"
    >
      X-axis
    </text>
  </g>
</svg>