<script>
  // @ts-nocheck
  import { createEventDispatcher } from "svelte";
  const dispatch = createEventDispatcher();

  export let value = 0;
  let progressEl;
  let mouseProgress = 0;

  function handleClick(e) {
    dispatch("change", (e.offsetX / progressEl.clientWidth) * 100);
  }
  function handleMouseEnter(e) {
    mouseProgress = (e.offsetX / progressEl.clientWidth) * 100;
  }
  function handleMouseLeave() {
    mouseProgress = 0;
  }
</script>

<div
  bind:this={progressEl}
  class="relative flex-1 h-2 bg-blue-100 bg-opacity-25 cursor-pointer"
  on:click={handleClick}
  on:mousemove={handleMouseEnter}
  on:mouseleave={handleMouseLeave}
>
  <div
    class="bg-red-200 absolute top-0 bottom-0 left-0 bg-opacity-50"
    style="width: {mouseProgress}%"
  />
  <div
    class="bg-red-500 absolute top-0 bottom-0 left-0"
    style="width: {value}%"
  />
</div>
