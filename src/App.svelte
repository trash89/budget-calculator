<script>
  import Navbar from "./Navbar.svelte";
  let fruits = ["apple", "orange", "lemon"];
</script>

<!-- <style></style> -->
<!-- <CSS/STYLING -->

<Navbar />

{#each fruits as item}
  <h1>fruit : {item}</h1>
{/each}
