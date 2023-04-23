<script>
  import { onMount } from "svelte";
  import Scatterplot from "/home/elkokk/datavis-technologies-handson/src/routes/02_exercises/02_basic_visualisations/exercise_21/Scatterplot.svelte";

  let data;
  let countriesFor1800;

  onMount(async () => {
    const response = await fetch("/home/elkokk/datavis-technologies-handson/static/data/gapminder.json");
    data = await response.json();
    countriesFor1800 = data.find((entry) => entry.year === 1800).countries;
  });
</script>

{#if !countriesFor1800}
  <p>Loading data...</p>
{:else}
  <Scatterplot {countriesFor1800} />
{/if}