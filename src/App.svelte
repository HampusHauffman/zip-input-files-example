<script lang="ts">
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

<main
  style="background-image: url(dot.svg)"
  class="bg-violet-800
  w-screen h-screen
  bg-[length:80px_80px]"
>
  <div class="flex items-center justify-center h-screen w-screen">
    <div
      class="absolute
        h-[50vh] w-1
        -translate-y-[34vh]
        -translate-x-16
        bg-black
        "
    />
    <div
      class="absolute
        h-[50vh] w-1
        -translate-y-[34vh]
        -translate-x-12
        opacity-25
        bg-black
        "
    />
    <div
      class="absolute
        h-[50vh] w-1
        -translate-y-[25vh]
        translate-x-28
        bg-black
        "
    />
    <div
      class="absolute
        h-[50vh] w-1
        -translate-y-[25vh]
        translate-x-32
        opacity-25
        bg-black
        "
    />
    <!-- Folders-->
    <div class="absolute group h-48 w-48 rotate-6 translate-x-24 translate-y-8">
      <!--Black bg-->
      <div
        class="absolute rounded-md
        h-48 w-48
        bg-black
        translate-x-3 translate-y-4
        transition
        group-hover:translate-x-2
        group-hover:rotate-6
        group-hover:translate-y-6"
      />
      <!--pink box bg-->
      <div
        style="background-image: url(dot.svg)"
        class="absolute rounded-md
        font-bold
        bg-[length:60px_60px]
        h-48 w-48
        ring-4 ring-black
        bg-green-600
        bg-center
        transition-all
        group-hover:h-52
        group-hover:w-52
        group-hover:-translate-x-2
        group-hover:-translate-y-2"
      >
        <!--input field-->
        <label
          for="file-upload"
          class="cursor-pointer flex flex-col items-center justify-center w-full h-full"
        >
          <p
            class="text-center font-black leading-[1.3rem] text-2xl drop-shadow-[0_3px_0px_rgba(0,0,0,1)]"
          >
            ZIP A<br />
            FOLDER
          </p>
          <input
            id="file-upload"
            type="file"
            accept="*"
            multiple
            class="hidden"
            bind:files
          />
        </label>
      </div>
    </div>

    <!--The other button-->
    <div
      class="absolute group h-48 w-48 -rotate-6 -translate-x-16 -translate-y-32"
    >
      <!--Black bg-->
      <div
        class="absolute rounded-md
        h-48 w-48
        bg-black
        translate-x-3 translate-y-3
        transition
        group-hover:translate-x-6
        group-hover:rotate-3
        group-hover:translate-y-6"
      />
      <!--pink box bg-->
      <div
        style="background-image: url(dot.svg)"
        class="absolute rounded-md
        font-bold
        bg-[length:30px_30px]
        h-48 w-48
        bg-pink-600
        bg-center
        ring-4 ring-black
        transition-all
        group-hover:h-52
        group-hover:w-52
        group-hover:-translate-x-2
        group-hover:-translate-y-2
        "
      >
        <!--input field-->
        <label
          for="file-upload"
          class="cursor-pointer flex flex-col items-center justify-center w-full h-full"
        >
          <p
            class="text-center font-black leading-[1.3rem] text-2xl drop-shadow-[0_3px_0px_rgba(0,0,0,1)]"
          >
            ZIP A<br />
            FILE
          </p>
          <input
            id="file-upload"
            type="file"
            accept="*"
            multiple
            class="hidden"
            bind:files
          />
        </label>
      </div>
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
