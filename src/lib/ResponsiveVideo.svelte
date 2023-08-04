<script>
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
  $: src = `/video/mobile_portrait.mp4`
  $: {
    if ($screenWidth <= breakpoint) {
      // mobile
      if ($screenHeight > $screenWidth * breakFactor) {
        // portrait
        src = `/video/mobile_portrait.mp4`;
      } else if ($screenWidth > $screenHeight * breakFactor) {
        // landscape
        src = `/video/mobile_landscape.mp4`;
      } else {
        // square
        src = `/video/mobile_square.mp4`;
      }
    } else {
      // desktop
      if ($screenHeight > $screenWidth * breakFactor) {
        // portrait
        src = `/video/desktop_portrait.mp4`;
      } else if ($screenWidth > $screenHeight * breakFactor) {
        // landscape
        src = `/video/desktop_landscape.mp4`;
      } else {
        // square
        src = `/video/desktop_square.mp4`;
      }
    }
  }
</script>

<video width="100%" src="{src}" loop autoplay muted>
  Your browser does not support the video tag.
</video>


<style>

</style>