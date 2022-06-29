<script>
  import Title from "./Title.svelte";
  export let name = "";
  export let amount = null;
  export let isEditing;
  export let handleSubmit;
  export let hideForm;
  //
  $: isEmpty = !name || !amount;
  //
</script>

<section class="form" on:submit|preventDefault={handleSubmit}>
  <Title title="add expense" />
  <form class="expense-form">
    <div class="form-control">
      <label for="name">name</label>
      <input type="text" id="name" bind:value={name} />
    </div>
    <div class="form-control">
      <label for="amount">amount</label>
      <input type="number" id="amount" bind:value={amount} />
    </div>
    {#if isEmpty}
      <p class="form-empty">please fill out all form fields</p>
    {/if}
    <button type="submit" class="btn btn-block" class:disabled={isEmpty} disabled={isEmpty}>
      {#if isEditing}edit expense{:else}add expense{/if}
    </button>
    <button type="button" class="close-btn" on:click={hideForm}>
      <i class="fas fa-times" />
    </button>
  </form>
</section>
