<script>
  let current = "";
</script>

<!-- We will cover buttons later on! -->
<button class="btn btn-secondary"
class:selected={current === "messi"}
on:click={() => (current = "messi")}>
  Messi
</button>

<button class="btn btn-secondary"
class:selected={current === "ronaldo"}
 on:click={() => (current = "ronaldo")}>
  Ronaldo
</button>

<button class="btn btn-secondary"
class:selected={current === "neymar"}
 on:click={() => (current = "neymar")}>
  Neymar
</button>

<style>
  .selected {
    background-color: #000bff;
    color: white;
  }
</style>