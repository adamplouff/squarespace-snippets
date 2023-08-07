<script>
  import { base } from "$app/paths";
  import { onMount } from 'svelte';

  // Define the breakpoint at which we'll switch videos
  const breakpoint = 1280;
  const breakFactor = 1.3

  // Create a store to hold the screen width
  import { writable } from 'svelte/store';
  const screenWidth = writable(0);
  const screenHeight = writable(0);

  // Function to update the screenWidth store based on the window width
  function updateScreenWidth() {
    screenWidth.set(window.innerWidth);
    screenHeight.set(window.innerHeight);
  }

  // Subscribe to screen width changes on component mount
  onMount(() => {
    updateScreenWidth();
    window.addEventListener('resize', updateScreenWidth);
    return () => window.removeEventListener('resize', updateScreenWidth);
  });

  // Check if the screen width is greater than or equal to the breakpoint
  $: isDesktop = $screenWidth >= breakpoint;

  // $: src = ($screenWidth >= breakpoint) ? `/video/desktop_landscape.mp4` : `/video/mobile_portrait.mp4`;
  $: src = `${ base }/video/mobile_portrait.mp4`
  $: {
    if ($screenWidth <= breakpoint) {
      // mobile
      if ($screenHeight > $screenWidth * breakFactor) {
        // portrait
        src = `${ base }/video/mobile_portrait.mp4`;
      } else if ($screenWidth > $screenHeight * breakFactor) {
        // landscape
        src = `${ base }/video/mobile_landscape.mp4`;
      } else {
        // square
        src = `${ base }/video/mobile_square.mp4`;
      }
    } else {
      // desktop
      if ($screenHeight > $screenWidth * breakFactor) {
        // portrait
        src = `${ base }/video/desktop_portrait.mp4`;
      } else if ($screenWidth > $screenHeight * breakFactor) {
        // landscape
        src = `${ base }/video/desktop_landscape.mp4`;
      } else {
        // square
        src = `${ base }/video/desktop_square.mp4`;
      }
    }
  }

  const clickZones = [
    {
      left: '20%',
      top: '10%',
      width: '20%',
      height: '10%',
      url: 'https://www.battleaxe.co/overlord',
    },
    {
      left: '70%',
      top: '17%',
      width: '20%',
      height: '30%',
      url: 'https://www.battleaxe.co/rubberhose',
    },
  ]
</script>

<div class="clickable">
  {#each clickZones as clickZone}
    <a href="{clickZone.url}">
      <div class="click-zone" style="
        margin-top: {clickZone.top}; 
        margin-left: {clickZone.left}; 
        width: {clickZone.width}; 
        height: {clickZone.height};"></div>
    </a>
  {/each}
  <!-- <div class="click-zone" style="width: {clickZone.width}; height: {clickZone.height} "></div> -->
    
</div>

<video width="100%" src="{src}" loop autoplay muted playsinline>
  Your browser does not support the video tag.
</video>

<style>
  .clickable {
    position: absolute;
    z-index: 10;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
  }
  .click-zone {
    position: absolute;
    width: 10%;
    cursor: pointer;
    background: rgba(255, 255, 255, 0.1);
    border: 1px solid rgba(255, 255, 255, 0.1);
  }
  video {
    position: absolute;
    top: 0;

  }
</style>