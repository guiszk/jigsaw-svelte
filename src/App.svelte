<script>
  import Jigsaw from './lib/Jigsaw.svelte'
  let jigsawSrc;
  let gameStart = false;
  let JigsawImg;
  const options = ['Easy', 'Medium', 'Hard'];
  let selected = options[0];
  import { num_rows } from './lib/stores.js';
  num_rows.update(n => 5);
  let num_rows_value;
  num_rows.subscribe(value => {
		num_rows_value = value;
	});

  function startGame () {
    if(jigsawSrc){
      gameStart = true;
      JigsawImg.style.display = "none";
    }
  }
  function setLevel () {
    if (selected === 'Easy'){
      num_rows.update(n => 5);
    } else if (selected === 'Medium') {
      num_rows.update(n => 7);
    } else if (selected === 'Hard') {
      num_rows.update(n => 10);
    }
  }
</script>

<main class="container">
  <h1>Create your own jigsaw!</h1>
  <form>
    <label for="img">Select image:</label>
    <input type="file" id="img" name="img" accept="image/*">
    <input type="submit" on:click|preventDefault={(e) => {
      // @ts-ignore
      let file = e.target.form[0].files[0]
      if(file){
        jigsawSrc = URL.createObjectURL(file)
      }
      console.log(jigsawSrc)
    }}>
  </form>
  <button on:click = {() => jigsawSrc = "https://picsum.photos/500?random="+ Math.floor(Math.random() * 1000)}>Solve random image</button>
  {#if (jigsawSrc)}
    <img src={jigsawSrc} bind:this={JigsawImg} alt="puzzle" width="700px">
  {/if}
  <select bind:value={selected} on:click={setLevel}>
    {#each options as option}
      <option value={option}>{option}</option>
    {/each}
  </select>

  {#if selected === 'Easy'}
    <p>Selected Easy ({num_rows_value} x {num_rows_value})</p>
  {:else if selected === 'Medium'}
    <p>Selected Medium ({num_rows_value} x {num_rows_value})</p>
  {:else if selected === 'Hard'}
    <p>Selected Hard ({num_rows_value} x {num_rows_value})</p>
  {/if}
  <button on:click={startGame}>Start</button>
</main>
{#if (gameStart)}
<Jigsaw img={JigsawImg}/>
{/if}

<style>
 main {
  margin-bottom: 2rem;
 }
</style>

