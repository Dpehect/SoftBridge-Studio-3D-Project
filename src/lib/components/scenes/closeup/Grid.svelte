<script>
  import { getContext } from "svelte";

  const image = getContext("images");

  const polaroids = [
    { src: "user_photo_1.jpg", rotate: -6, left: "80px", top: "70px" },
    { src: "user_photo_2.jpg", rotate: 5, left: "310px", top: "40px" },
    { src: "user_photo_3.jpg", rotate: -4, left: "540px", top: "90px" },
    { src: "user_photo_4.jpg", rotate: 6, left: "770px", top: "50px" },
    { src: "user_photo_5.jpg", rotate: -5, left: "1000px", top: "80px" },
  ];

  let isDown = $state(false);
  let startX = $state(0);
  let scrollLeft = $state(0);
  let parentEl = $state(null);

  function handleMouseDown(e) {
    if (e.button !== 0) return; // Only drag with left click
    isDown = true;
    startX = e.pageX - parentEl.offsetLeft;
    scrollLeft = parentEl.scrollLeft;
  }

  function handleMouseLeave() {
    isDown = false;
  }

  function handleMouseUp() {
    isDown = false;
  }

  function handleMouseMove(e) {
    if (!isDown) return;
    e.preventDefault();
    const x = e.pageX - parentEl.offsetLeft;
    const walk = (x - startX) * 1.5; // Drag scroll speed
    parentEl.scrollLeft = scrollLeft - walk;
  }
</script>

<!-- svelte-ignore a11y_no_static_element_interactions -->
<div 
  bind:this={parentEl}
  class="grid-div" 
  class:grabbing={isDown}
  onmousedown={handleMouseDown}
  onmouseleave={handleMouseLeave}
  onmouseup={handleMouseUp}
  onmousemove={handleMouseMove}
>
  <div class="photo-board">
    <div class="wire-grid-vertical" style="left: 16%"></div>
    <div class="wire-grid-vertical" style="left: 32%"></div>
    <div class="wire-grid-vertical" style="left: 48%"></div>
    <div class="wire-grid-vertical" style="left: 64%"></div>
    <div class="wire-grid-vertical" style="left: 80%"></div>
    <div class="wire-grid-horizontal" style="top: 25%"></div>
    <div class="wire-grid-horizontal" style="top: 50%"></div>
    <div class="wire-grid-horizontal" style="top: 75%"></div>

    {#each polaroids as photo}
      <!-- svelte-ignore a11y_no_static_element_interactions -->
      <div 
        class="polaroid-wrapper" 
        style="left: {photo.left}; top: {photo.top}; transform: rotate({photo.rotate}deg);"
        ondragstart={(e) => e.preventDefault()}
      >
        <div class="peg"></div>
        <div class="polaroid">
          <img src={image[photo.src]} alt="Memory" draggable="false" />
        </div>
      </div>
    {/each}
  </div>
</div>

<style>
  .grid-div {
    position: relative;
    width: 100%;
    height: 100%;
    overflow-x: auto;
    overflow-y: hidden;
    background-color: #2b1814;
    background-image: 
      linear-gradient(335deg, #1b0c09 23px, transparent 23px),
      linear-gradient(155deg, #1b0c09 23px, transparent 23px),
      linear-gradient(335deg, #1b0c09 23px, transparent 23px),
      linear-gradient(155deg, #1b0c09 23px, transparent 23px);
    background-size: 58px 58px;
    background-position: 0px 2px, 4px 35px, 29px 31px, 34px 6px;
    display: flex;
    justify-content: flex-start;
    align-items: center;
    cursor: grab;
    user-select: none;
    -webkit-overflow-scrolling: touch;
    padding: 0 40px;
  }

  .grid-div.grabbing {
    cursor: grabbing;
  }

  /* Custom scrollbar styling */
  .grid-div::-webkit-scrollbar {
    height: 8px;
  }
  .grid-div::-webkit-scrollbar-track {
    background: rgba(0, 0, 0, 0.3);
  }
  .grid-div::-webkit-scrollbar-thumb {
    background: rgba(181, 137, 61, 0.4);
    border-radius: 4px;
    transition: background-color 0.2s ease;
  }
  .grid-div::-webkit-scrollbar-thumb:hover {
    background: rgba(181, 137, 61, 0.7);
  }

  .photo-board {
    position: relative;
    width: 1250px;
    height: 450px;
    background: rgba(0, 0, 0, 0.15);
    border: 4px solid #1a0f0d;
    box-shadow: inset 0 0 20px rgba(0, 0, 0, 0.6);
    flex-shrink: 0;
    margin: auto;
  }

  .wire-grid-vertical {
    position: absolute;
    top: 0;
    bottom: 0;
    width: 2px;
    background: rgba(100, 100, 100, 0.35);
    box-shadow: 1px 0 2px rgba(0, 0, 0, 0.5);
  }

  .wire-grid-horizontal {
    position: absolute;
    left: 0;
    right: 0;
    height: 2px;
    background: rgba(100, 100, 100, 0.35);
    box-shadow: 0 1px 2px rgba(0, 0, 0, 0.5);
  }

  .polaroid-wrapper {
    position: absolute;
    width: 140px;
    display: flex;
    flex-direction: column;
    align-items: center;
    transition: transform 0.25s cubic-bezier(0.175, 0.885, 0.32, 1.275), z-index 0.2s ease;
    will-change: transform;
  }

  .polaroid-wrapper:hover {
    transform: scale(1.35) rotate(0deg) !important;
    z-index: 10;
  }

  .peg {
    width: 8px;
    height: 20px;
    background: #b5893d;
    border-radius: 1px;
    box-shadow: 1px 2px 3px rgba(0, 0, 0, 0.4);
    z-index: 2;
    margin-bottom: -5px;
  }

  .polaroid {
    background: #fbfbf9;
    padding: 6px 6px 18px 6px;
    box-shadow: 0 6px 12px rgba(0, 0, 0, 0.45);
    border-radius: 1px;
    width: 100%;
    border: 1px solid rgba(0, 0, 0, 0.05);
  }

  .polaroid img {
    width: 100%;
    height: 100%;
    object-fit: cover;
    aspect-ratio: 1;
    border: 1px solid rgba(0, 0, 0, 0.06);
    user-select: none;
    pointer-events: none;
  }
</style>
