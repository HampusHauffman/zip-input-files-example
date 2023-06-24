<script lang="ts">
  export let files: FileList;
  export let classes: string;
  export let box_rot: string = "";
  export let folder: boolean;

  function setAttributeWebkitdirectory(node: HTMLInputElement) {
    if (folder) {
      node.setAttribute("webkitdirectory", "");
    }
  }
</script>

<div class="flex items-center justify-center">
  <!-- rope -->
  <div
    class="absolute
        h-[60vh] w-1
        -translate-y-[27vh]
        bg-black
        "
  />
  <div
    class="absolute
        h-[60vh] w-1
        -translate-y-[27vh]
        translate-x-4
        opacity-25
        bg-black
        "
  />

  <div
    class="static group h-48 w-48
    {box_rot} "
  >
    <!--Black bg-->
    <div
      class="absolute rounded-md
        h-48 w-48
        bg-black
        translate-x-3 translate-y-3
        transition-all
        transform-gpu
        group-hover:rotate-3
        group-hover:translate-x-6
        group-hover:translate-y-6"
    />
    <!--The box-->
    <div
      style="background-image: url(dot.svg)"
      class="absolute rounded-md
        font-bold
        h-48 w-48
        {classes}
        bg-center
        ring-4 ring-black
        transition-all
        transform-gpu
        group-hover:h-52
        group-hover:w-52
        group-hover:-translate-x-2
        group-hover:-translate-y-2
        "
    >
      <!--input field-->
      <label
        for="{folder}file-upload"
        class="cursor-pointer flex flex-col items-center justify-center w-full h-full"
      >
        <p
          class="text-center font-black leading-[1.3rem] text-2xl drop-shadow-[0_3px_0px_rgba(0,0,0,1)]"
        >
          ZIP A<br />
          {#if folder}
            FOLDER
          {:else}
            FILE
          {/if}
        </p>
        <input
          id="{folder}file-upload"
          type="file"
          accept="*"
          multiple
          class="hidden"
          use:setAttributeWebkitdirectory
          bind:files
        />
      </label>
    </div>

    <div
      style="background-image: url(zip.svg);"
      class="absolute w-full h-full bg-[length:50px_50px] bg-no-repeat bg-right-top
      fill-orange-600
      pointer-events-none
      transition-all
      group-hover:-translate-y-2
      "
    />
  </div>
</div>
