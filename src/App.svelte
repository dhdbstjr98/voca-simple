<script>
  import Register from "./component/Register.svelte";
  import WordList from "./component/WordList.svelte";
</script>

<style>
  .main {
    width: 100%;
    max-width: 800px;
    margin: 0 auto;
    padding: 10px;
  }
</style>

<div class="main">
  <Register />
  <WordList />
</div>
