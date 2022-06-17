<script>
  // @ts-nocheck
  import VideoProgress from "./VideoProgress.svelte";

  export let src = "";
  export let width = "auto";
  export let controls = false;
  let videoEl;
  let playing = false;
  let duration = 0;
  let currentTime = 0;
  let progressWidth = 0;

  function playPause() {
    playing ? videoEl.pause() : videoEl.play();
  }
  function handlePlayerReady() {
    duration = videoEl.duration;
  }
  function handleTimeUpdate() {
    currentTime = videoEl.currentTime;
  }
  function handleProgressClick({ detail }) {
    currentTime = duration * (detail / 100);
    videoEl.currentTime = duration * (detail / 100);
  }

  $: progressWidth = (currentTime / duration) * 100;
</script>

<div class="relative">
  <video
    bind:this={videoEl}
    {controls}
    {width}
    on:play={() => (playing = true)}
    on:pause={() => (playing = false)}
    on:loadedmetadata={handlePlayerReady}
    on:timeupdate={handleTimeUpdate}
    on:click={!controls ? playPause : null}
  >
    <source {src} />
    <track kind="captions" />
  </video>
  {#if !controls}
    <div class="absolute flex items-center gap-2 bottom-0 w-full z-10">
      <span class="cursor-pointer rounded-full bg-red-100" on:click={playPause}>
        {playing ? "pause" : "play"}
      </span>
      <VideoProgress value={progressWidth} on:change={handleProgressClick} />
    </div>
  {/if}
</div>

<div>
  {progressWidth}
</div>
