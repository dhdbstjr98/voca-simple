<script>
  import { words } from "../store";
  export let word = "";
  export let meaning = "";
  export let idx = -1;

  const deleteWord = () => {
    if (idx === -1) return;
    words.set($words.filter((_, i) => i != idx));
  };

  const speakWord = () => {
    new Audio(
      `https://tts-translate.kakao.com/read?format=mpeg&lang=en&txt=${word}`
    ).play();
  };
</script>

<style>
  .collapsible-body {
    background-color: #fff;
  }
  .buttons {
    text-align: right;
    margin-top: -1rem;
    margin-right: -1rem;
    margin-bottom: 1em;
  }
  .buttons button {
    margin-left: 10px;
  }

  .collapsible-header {
    padding-left: 4.3rem;
    position: relative;
  }

  .collapsible-header .btn-play {
    position: absolute;
    top: 0;
    left: 0;
    width: 4rem;
    height: 100%;
    border: 0;
    background: unset;
  }

  .collapsible-header .btn-play .material-icons {
    margin-right: 0;
    font-size: 1.8em;
  }
</style>

<li>
  <div class="collapsible-header">
    <button class="btn-play" on:click|stopPropagation={speakWord}>
      <i class="material-icons orange-text text-darken-4">play_circle_filled</i>
    </button>
    {word}
  </div>
  <div class="collapsible-body">
    <div class="buttons">
      <button
        type="button"
        class="btn-floating waves-effect waves-light red accent-3"
        on:click={deleteWord}>
        <i class="material-icons">delete</i>
      </button>
      <button
        type="button"
        class="btn-floating waves-effect waves-light orange accent-4"
        on:click|stopPropagation={speakWord}>
        <i class="material-icons">play_arrow</i>
      </button>
    </div>
    <div class="meaning">{meaning}</div>
  </div>
</li>
