<script>
  let show_register = false;
  let word = "";
  let meaning = "";
  import { words } from "../store";

  const register = () => {
    if (!word) {
      M.toast({ html: "단어를 입력해주세요." });
      return;
    }
    if (!meaning) {
      M.toast({ html: "뜻을 입력해주세요." });
      return;
    }
    words.set([...$words, { word, meaning }]);
    word = "";
    meaning = "";
    show_register = false;
  };
</script>

<style>
  .pre {
    background-color: #81c784;
    height: 50px;
    border-radius: 5px;
    box-sizing: border-box;
    display: flex;
    align-items: center;
    color: #fff3e0;
    font-size: 1.1em;
  }

  .pre-inner {
    padding-left: 10px;
  }

  .pre-inner i {
    margin-top: -3px;
  }

  .pre-inner i,
  .pre-inner span {
    vertical-align: middle;
  }

  .register {
    background-color: #fff;
    border-radius: 5px;
    padding: 30px;
  }

  .buttons {
    text-align: right;
  }
</style>

{#if !show_register}
  <div
    class="pre z-depth-2"
    on:click={() => {
      show_register = true;
    }}>
    <div class="pre-inner">
      <i class="material-icons">add</i>
      <span>단어를 입력해주세요.</span>
    </div>
  </div>
{:else}
  <div class="register z-depth-2">
    <div class="input-field">
      <input id="word" type="text" bind:value={word} />
      <label for="word">단어</label>
    </div>
    <div class="input-field">
      <input id="meaning" type="text" bind:value={meaning} />
      <label for="meaning">뜻</label>
    </div>
    <div class="buttons">
      <button
        type="button"
        class="btn waves-effect waves-light red accent-1"
        on:click={() => {
          show_register = false;
        }}>
        취소
      </button>
      <button
        type="button"
        class="btn waves-effect waves-light orange accent-4"
        on:click={register}>
        등록
      </button>
    </div>
  </div>
{/if}
