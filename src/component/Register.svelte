<script>
  import { tick } from "svelte";
  import { words } from "../store";
  import httpJsonp from "http-jsonp";
  import { v4 } from "uuid";

  let show_register = false;
  let word = "";
  let meaning = "";
  let ref_word = null;
  let ref_meaning = null;

  const toggleRegister = async () => {
    show_register = !show_register;

    await tick();

    if (show_register) {
      ref_word.focus();
    } else {
      word = "";
      meaning = "";
    }
  };

  const focusWord = () => {
    ref_word.focus();
  };

  const register = () => {
    if (!word) {
      M.toast({ html: "단어를 입력해주세요." });
      return;
    }
    if (!meaning) {
      M.toast({ html: "뜻을 입력해주세요." });
      return;
    }
    words.set([{ word, meaning }, ...$words]);
    word = "";
    meaning = "";
    show_register = false;
  };

  const speakWord = () => {
    new Audio(
      `https://tts-translate.kakao.com/read?format=mpeg&lang=en&txt=${word}`
    ).play();
  };

  const getMeaning = () => {
    if (!word) {
      M.toast({ html: "단어를 입력해주세요." });
      return;
    }

    httpJsonp({
      url: `https://suggest.dic.daum.net/language/v1/search.json?cate=lan&q=${word}`,
      params: {
        callback: `callback${v4().substring(0, 8)}`
      },
      callbackProp: "callback",
      callback: data => {
        if (
          !data ||
          !data.items ||
          !data.items.lan ||
          data.items.lan.length === 0
        ) {
          M.toast({ html: "단어를 찾을 수 없습니다." });
        }

        [, word, meaning] = data.items.lan[0].item.split("|");

        speakWord();
        ref_meaning.focus();
      }
    });
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

  .word-wrapper {
    padding-right: 6rem;
    position: relative;
  }

  .word-wrapper button {
    position: absolute;
    right: 0;
    bottom: 8px;
    width: 5rem;
    box-sizing: border-box;
  }
</style>

{#if !show_register}
  <div class="pre z-depth-2" on:click={toggleRegister}>
    <div class="pre-inner">
      <i class="material-icons">add</i>
      <span>단어를 입력해주세요.</span>
    </div>
  </div>
{:else}
  <div class="register z-depth-2">
    <div class="word-wrapper">
      <div class="input-field">
        <input id="word" type="text" bind:value={word} bind:this={ref_word} />
        <label for="word">단어</label>
      </div>
      <button
        type="button"
        class="btn waves-effect waves-light red accent-1"
        on:click={getMeaning}>
        자동
      </button>
    </div>
    <div class="input-field">
      <input
        id="meaning"
        type="text"
        bind:value={meaning}
        bind:this={ref_meaning} />
      <label for="meaning">뜻</label>
    </div>
    <div class="buttons">
      <button
        type="button"
        class="btn waves-effect waves-light red accent-1"
        on:click={toggleRegister}>
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
