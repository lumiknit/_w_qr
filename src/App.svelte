<script>
  import "@picocss/pico";

  import Gen from "./Gen.svelte";
  import FromImage from "./FromImage.svelte";
  import FromCamera from "./FromCamera.svelte";

  const MODES = {
    GEN: "Gen",
    FROM_IMAGE: "From Image",
    FROM_CAMERA: "From Camera",
  }

  let mode = MODES.GEN;

  const switchMode = (m) => () => {
    mode = m;
  };

</script>

<div class="container">
  <header>
    <h1> QR Util </h1>
    QR code image generator & scanner.
  </header>

  <fieldset role="group">
    {#each Object.values(MODES) as m}
      <button class={mode !== m ? "outline" : ""} on:click={switchMode(m)}> {m}</button>
    {/each}
  </fieldset>

  <main>
      {#if mode === MODES.GEN}
        <Gen />
      {:else if mode === MODES.FROM_IMAGE}
        <FromImage />
      {:else if mode === MODES.FROM_CAMERA}
        <FromCamera />
      {/if}
  </main>
</div>

<style>
</style>
