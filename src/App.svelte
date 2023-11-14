<script lang="ts">
  import { HfInference } from "@huggingface/inference"; // importujemy potrzebne narzędzie z biblioteki HuggingFace
  let userInput: string;
  let imagePromise: Promise<string>;

  const hf = new HfInference(import.meta.env.VITE_HUGGING_FACE_ACCESS_TOKEN); // konfigurujemy narzędzie z naszym kluczem dostępu

  async function ask() {
    const blob = await hf.textToImage({
      // interesujący nas sposób interakcji z danym modelem - w tym wypadku wpisany tekst zamieniany na wygenerowany obraz
      model: "stabilityai/stable-diffusion-xl-base-1.0", // konkretny model ze strony HuggingFace
      inputs: userInput, // dane wejściowe użytkownika
    });
    return URL.createObjectURL(blob);
  }

  function generateImage() {
    imagePromise = ask();
  }
</script>

<main>
    <input bind:value={userInput} />
    <button on:click={generateImage}>Generate</button>
    {#if imagePromise}
      {#await imagePromise}
        Loading...
      {:then result}
        <!-- svelte-ignore a11y-img-redundant-alt -->
        <img width=320 height=320 src={result} alt="Ai generated image"/>
      {:catch}
        Ojoj niepykło ci tym razem XD ~otóż nie tym razem ~
      {/await}
    {:else}
      No image generated
    {/if}
</main>

<style>
    main
    {
      background-color: cadetblue;
      position: absolute;
      top: 0;
      left: 0;
      right: 0;
      bottom: 0;
      width: 100%;
      height: 100%;
    }
    input
    {
      pos
      padding: 4px;
      border-radius: 1000px;
      text-align: center;
      background-color: rgb(207, 238, 252)
    }
    button
    {
      padding: 4px;
      border-radius: 1000px;
      background-color: rgb(156, 224, 252)
    }
</style>