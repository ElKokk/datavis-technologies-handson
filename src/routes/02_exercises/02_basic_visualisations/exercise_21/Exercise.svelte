<script>
  import { onMount } from 'svelte';
  import Scatterplot from '/home/elkokk/datavis-technologies-handson/src/routes/02_exercises/02_basic_visualisations/exercise_21/Scatterplot.svelte';

  let data;
  let countries1800;

  onMount(async () => {
    try {
      const response = await fetch('/home/elkokk/datavis-technologies-handson/static/data/gapminder.json');
      data = await response.json();
      console.log('Data:', data);
      countries1800 = data.find(d => d.year === '1800').countries;
    } catch (error) {
      console.error('Error fetching data:', error);
    }
  });
</script>

{#if !countries1800}
  <p>Loading data...</p>
{:else}
  <Scatterplot {countries1800} />
{/if}