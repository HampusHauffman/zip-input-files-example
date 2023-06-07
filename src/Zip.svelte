<script lang="ts">
  export let files: FileList;
  export let classes: string;
  export let box_rot: string = "";
</script>

<div class="flex items-center justify-center">
  <div
    class="absolute
        h-[50vh] w-1
        -translate-y-[34vh]
        bg-black
        "
  />
  <div
    class="absolute
        h-[50vh] w-1
        -translate-y-[34vh]
        translate-x-4
        opacity-25
        bg-black
        "
  />

  <div class="absolute group h-48 w-48  {box_rot} ">
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
    <!--pink box bg-->
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
</div>
