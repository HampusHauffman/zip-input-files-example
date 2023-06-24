<script lang="ts">
  export let items: string[];

  let activeTab = items[0];
</script>

<div class="absolute top-0 left-0 ml-6 mt-6 grid grid-cols-1">
  {#each items as item}
    <a
      href={item}
      download="zipped_files.zip"
      class="mt-2 p-2 border-black border-4 rounded bg-yellow-600 font-bold"
    >
      <p
        class="text-center font-black leading-[1.3rem] text-2xl drop-shadow-[0_3px_0px_rgba(0,0,0,1)]"
      >
        DOWNLOAD ZIPPED FILE
      </p>
    </a>
  {/each}
</div>
