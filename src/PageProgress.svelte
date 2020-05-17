<script>
  let width = "0%"

  // props with default values
  export let color = "skyblue"
  export let height = "4px"

  const { style } = document.documentElement

  $: style.setProperty("--spp-color", color)
  $: style.setProperty("--spp-height", height)
  $: style.setProperty("--spp-width", width)

  const watchScrolling = () => {
    const { scrollHeight, clientHeight, scrollTop } = document.documentElement
    const winScroll = document.body.scrollTop || scrollTop
    const winHeight = scrollHeight - clientHeight
    const scrolled = `${(winScroll / winHeight) * 100}%`
    if (winHeight > 0) {
      width = scrolled
    } else {
      width = 0
    }
  }

  let timer
  const debounce = (fn) => {
    clearTimeout(timer)
    timer = setTimeout(fn, 100)
  }

  const watchScrollingDebounce = () => debounce(watchScrolling)
</script>

<style  >
  .svelte-page-progress {
    margin: 0;
    padding: 0;
    position: fixed;
    top: 0;
    left: 0;
    z-index: 999;
    background: var(--spp-color);
    height: var(--spp-height);
    width: var(--spp-width);
    transition: width 200ms ease-out;
  }
</style>

<div class="svelte-page-progress"></div>
<svelte:window on:scroll={watchScrollingDebounce} />
