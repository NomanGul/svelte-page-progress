<script>
  let width = "0%"

  // props with default values
  export let color = "skyblue";
  export let height = "4px";
  export let position = "top";
  export let opacity = 1;
  const { style } = document.documentElement

  $: style.setProperty("--spp-color", color)
  $: style.setProperty("--spp-height", height)
  $: style.setProperty("--spp-width", width)
  $: style.setProperty("--spp-opacity", opacity)
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
    z-index: 999999;
    background: var(--spp-color);
    height: var(--spp-height);
    width: var(--spp-width);
    opacity: var(--spp-opacity);
    transition: width 200ms ease-out;
  }
  .top{
    margin: 0;
    padding: 0;
    top: 0;
    left: 0;
  }

  .bottom{
      bottom: 0px;
      left: 0px;
  }
</style>

<div class="svelte-page-progress {position}"></div>
<svelte:window on:scroll={watchScrollingDebounce} />
