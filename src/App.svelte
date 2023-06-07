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

  // Start countdown when file_drag is set to true
  $: if (file_drag) {
    console.log("dragging");
  }

  $: if (file_drag) {
    countdown = 1;
    setInterval(() => {
      countdown--;
      if (countdown === 0) {
        file_drag = false; // Set file_drag to false when countdown reaches zero
      }
    }, 1000); // Update the countdown every second
  }
</script>

<main>
  <div
    id="all"
    class="absolute h-screen w-screen"
    on:drop={(e) => {
      e.preventDefault();
      files = e.dataTransfer.files;
    }}
    on:dragover={(e) => {
      e.preventDefault();
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
        scale_on_drag={true}
      />
    </div>

    <!-- show loading when promis is waiting-->
  </div>
  {#each zz as item}
    <a href={item} download="filename.zip">{item}</a>
  {/each}
</main>

<style>
</style>
