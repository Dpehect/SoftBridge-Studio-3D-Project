<script>
  import Panzoom from "@panzoom/panzoom";
  import { getContext } from "svelte";

  const image = getContext("images");

  let gridContainer = $state(null);

  $effect(async () => {
    if (!gridContainer) return;

    const panzoom = Panzoom(gridContainer, {
      contain: "outside",
      startScale: 1.5,
      minScale: 0.8,
      maxScale: 3,
    });
    
    const wheelHandler = (e) => panzoom.zoomWithWheel(e);
    gridContainer.parentElement.addEventListener("wheel", wheelHandler);

    return () => {
      if (gridContainer && gridContainer.parentElement) {
        gridContainer.parentElement.removeEventListener("wheel", wheelHandler);
      }
    };
  });

  const polaroids = [
    { src: "user_photo_1.jpg", rotate: -6, left: "12%", top: "15%" },
    { src: "user_photo_2.jpg", rotate: 5, left: "42%", top: "10%" },
    { src: "user_photo_3.jpg", rotate: -4, left: "72%", top: "18%" },
    { src: "user_photo_4.jpg", rotate: 6, left: "27%", top: "52%" },
    { src: "user_photo_5.jpg", rotate: -5, left: "57%", top: "48%" },
  ];
</script>

<div class="grid-div">
  <div bind:this={gridContainer} class="photo-board">
    <div class="wire-grid-vertical" style="left: 20%"></div>
    <div class="wire-grid-vertical" style="left: 40%"></div>
    <div class="wire-grid-vertical" style="left: 60%"></div>
    <div class="wire-grid-vertical" style="left: 80%"></div>
    <div class="wire-grid-horizontal" style="top: 25%"></div>
    <div class="wire-grid-horizontal" style="top: 50%"></div>
    <div class="wire-grid-horizontal" style="top: 75%"></div>

    {#each polaroids as photo}
      <div 
        class="polaroid-wrapper" 
        style="left: {photo.left}; top: {photo.top}; transform: rotate({photo.rotate}deg);"
      >
        <div class="peg"></div>
        <div class="polaroid">
          <img src={image[photo.src]} alt="Memory" />
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
    overflow: hidden;
    background-color: #2b1814;
    background-image: 
      linear-gradient(335deg, #1b0c09 23px, transparent 23px),
      linear-gradient(155deg, #1b0c09 23px, transparent 23px),
      linear-gradient(335deg, #1b0c09 23px, transparent 23px),
      linear-gradient(155deg, #1b0c09 23px, transparent 23px);
    background-size: 58px 58px;
    background-position: 0px 2px, 4px 35px, 29px 31px, 34px 6px;
    display: flex;
    justify-content: center;
    align-items: center;
  }

  .photo-board {
    position: relative;
    width: 900px;
    height: 600px;
    background: rgba(0, 0, 0, 0.15);
    border: 4px solid #1a0f0d;
    box-shadow: inset 0 0 20px rgba(0, 0, 0, 0.6);
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
    cursor: grab;
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
  }
</style>
