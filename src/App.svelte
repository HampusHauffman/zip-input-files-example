<script lang="ts">
  import Zip from "./Zip.svelte";
  import { WasmZip } from "wasm-zip";
  let files: FileList;
  let loading = false;
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
</script>

<main>
  <div
    style="background-image: url(dot.svg)"
    class="
    bg-violet-800 rounded-3xl ring-inset ring-yellow-600 ring-[1rem]
    flex items-center justify-center
    h-screen w-screen
    bg-[length:80px_80px]"
  >
    <!-- Folders-->
    <div
      class="transition-all absolute translate-y-16 translate-x-16 sm:translate-x-24"
    >
      <Zip {files} classes="bg-green-600 bg-[length:60px_60px]" box_rot="rotate-6"/>
    </div>

    <div
      class="transition-all absolute -translate-y-24 -translate-x-16"
    >
      <Zip {files} classes="bg-pink-600 bg-[length:24px_24px]" box_rot="-rotate-6"/>
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
