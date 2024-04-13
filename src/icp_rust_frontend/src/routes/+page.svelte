<script lang="ts">
  import "../index.scss";
  import { backend } from "$lib/canisters";
  import { onMount } from "svelte";

  let value = "";
  let greeting = "";
  let posts = "";

  async function onSubmit() {
    const a = await backend.upload(value);
    greeting = a;
    posts = backend.get();

    return false;
  }

  onMount(async () => {
    posts = backend.get();
  });
</script>

<main>
  <img src="/logo2.svg" alt="DFINITY logo" />
  <br />
  <br />
  <form action="#" on:submit|preventDefault={onSubmit}>
    <label for="name">Enter your name: &nbsp;</label>
    <input id="name" alt="Name" type="text" bind:value />
    <button type="submit">Click Me!</button>
  </form>
  <section id="greeting">{greeting}</section>
  <section>{posts}</section>

  {#await posts}
    <p>Fetching posts...</p>
  {:then posts}
    <ul>
      {#each posts as post}
        <li>{post}</li>
      {/each}
    </ul>
  {:catch error}
    <p style="color: red">{error.message}</p>
  {/await}
</main>
