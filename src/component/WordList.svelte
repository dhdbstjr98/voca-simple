<script>
  import { afterUpdate } from "svelte";
  import Word from "./Word.svelte";
  import { words } from "../store";

  afterUpdate(() => {
    M.Collapsible.init(document.querySelectorAll(".collapsible"), {});
  });
</script>

<style>
  .collapsible,
  .no-word {
    margin-top: 2em;
  }

  .collapsible {
    font-size: 1.6em;
  }

  .no-word {
    padding: 1rem;
    background-color: #fff;
  }
</style>

{#if $words.length > 0}
  <ul class="collapsible">
    {#each $words as word, i}
      <Word word={word.word} meaning={word.meaning} idx={i} />
    {/each}
  </ul>
{:else}
  <div class="no-word z-depth-1">등록된 단어가 없습니다.</div>
{/if}
