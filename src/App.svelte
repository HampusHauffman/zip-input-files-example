<script lang="ts">
  import DownloadLink from "./DownloadLink.svelte";
  import Zip from "./Zip.svelte";
  import init, {WasmZip} from "../node_modules/wasm-zip/wasm_zip.js";
  let loading = false;
  let files: FileList;
  let wasm_zip ;
  init().then((a ) => {
    wasm_zip = new WasmZip();
  });
  let stored_zipped_files: string[] = [];

  $: if (files) {
    //16:20
    let startTime = new Date().getTime();
    loading = true;

    let zipped_files: Promise<string> = wasm_zip.zip(files);

    zipped_files.then((z) => {
      console.log(z);
      stored_zipped_files = wasm_zip.get_zip();
      loading = false;
      let endTime = new Date().getTime();
      let timeDiff = endTime - startTime;
      console.log(timeDiff);
    });
  }
</script>

<main>
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
        classes="bg-green-600 bg-[length:60px_60px]"
        box_rot="rotate-6"
        folder={false}
        bind:files
      />
    </div>

    <div class="transition-all absolute -translate-y-24 -translate-x-16">
      <Zip
        classes="bg-pink-600 bg-[length:24px_24px]"
        box_rot="-rotate-6"
        folder={true}
        bind:files
      />
    </div>

    <!-- show loading when promis is waiting-->
    {#if files}
      <div class="absolute top-0 left-0 ml-6 mt-6 grid grid-cols-1">
        <p class="text-2xl font-bold text-center">Loading...</p>
      </div>
    {/if}
  </div>
  <DownloadLink items={stored_zipped_files} />
</main>

<style>
</style>
