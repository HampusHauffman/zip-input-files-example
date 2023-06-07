<script lang="ts">
  import { onMount } from "svelte";
  import Zip from "./Zip.svelte";
  import { WasmZip } from "wasm-zip";
  let file_drag = false;
  let loading = false;
  let files: FileList;
  let zipped_files: Promise<string>;
  let wasm_zip = new WasmZip();
  let zz: string[] = [];

  $: if (files) {
    console.log(files);
    loading == true;
    let zipped_files: Promise<string> = wasm_zip.zip(files);
    zipped_files.then((z) => {
      console.log(z);
      zz = wasm_zip.get_zip();
      loading == false;
    });

    for (const file of files) {
      console.log(`${file.name}: ${file.size} bytes`);
    }
  }

  let countdown = 1; // Countdown duration in seconds

  let countdownInterval: number | undefined;

  function startCountdown() {
    countdown = 1; // Reset the countdown duration
    countdownInterval = setInterval(() => {
      countdown--;
      if (countdown === 0) {
        file_drag = false; // Set file_drag to false when countdown reaches zero
        clearInterval(countdownInterval); // Clear the countdown interval
      }
    }, 1000); // Update the countdown every second
  }

  // Start countdown when file_drag is set to true
  $: if (file_drag) {
    startCountdown();
  }
</script>

<main>
  <div
    id="all"
    class="absolute h-screen w-screen"
    on:dragover={(e) => {
      file_drag = true;
    }}
  />
  <div
    style="background-image: url(dot.svg)"
    class="
    bg-violet-800 rounded-3xl ring-inset ring-yellow-600 ring-[1rem]
    flex items-center justify-center
    h-screen w-screen
    bg-[length:80px_80px]
    "
  >
    <!-- Folders-->
    <div
      class="transition-all absolute translate-y-16 translate-x-16 sm:translate-x-24"
    >
      <Zip
        {files}
        classes="bg-green-600 bg-[length:60px_60px]"
        box_rot="rotate-6"
      />
    </div>

    <div class="transition-all absolute -translate-y-24 -translate-x-16">
      <Zip
        {files}
        classes="bg-pink-600 bg-[length:24px_24px]"
        box_rot="-rotate-6"
        max={file_drag}
      />
    </div>

    <!-- show loading when promis is waiting-->
    {#if loading}
      {#await zipped_files then n}
        <div class="absolute flex flex-col items-center justify-center">
          <p class="text-center font-black leading-[1.3rem] text-2xl">
            Loading...
          </p>
        </div>
      {/await}
    {/if}

    {#each zz as item}
      <a href={item} download="filename.zip">{item}</a>
    {/each}
  </div>
</main>

<style>
</style>
