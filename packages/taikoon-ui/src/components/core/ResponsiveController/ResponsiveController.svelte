<script lang="ts">
  import { onDestroy, onMount } from 'svelte';

  // This component will help us to programmatically do the same as
  // CSS media queries. We can use it to show/hide elements or render
  // different components based on whether or not the size is desktop
  // or larger
  const isDesktopMediaQuery = window.matchMedia('(min-width: 1024px)');
  const isMobileMediaQuery = window.matchMedia('(max-width: 640px)');

  let isDesktop: boolean;
  let isMobile: boolean;

  export let windowSize: 'sm' | 'md' | 'lg' = 'md';

  function isDesktopQueryHandler(event: MediaQueryListEvent) {
    isDesktop = event.matches;
    isDesktop ? (windowSize = 'lg') : (windowSize = 'md');
  }

  function isMobileQueryHandler(event: MediaQueryListEvent) {
    isMobile = event.matches;
    isMobile ? (windowSize = 'sm') : (windowSize = 'md');
  }

  onMount(() => {
    isDesktop = isDesktopMediaQuery.matches;
    isMobile = isMobileMediaQuery.matches;
    isDesktopMediaQuery.addEventListener('change', isDesktopQueryHandler);
    isMobileMediaQuery.addEventListener('change', isMobileQueryHandler);
    //assign starting value
    windowSize = window.innerWidth > 1024 ? 'lg' : window.innerWidth < 640 ? 'sm' : 'md';
  });

  onDestroy(() => {
    isDesktopMediaQuery.removeEventListener('change', isDesktopQueryHandler);
    isMobileMediaQuery.removeEventListener('change', isMobileQueryHandler);
  });
</script>
