<script>
  function shuffle(arr) {
    for (let i = arr.length - 1; i > 0; i--) {
      const j = Math.floor(Math.random() * (i + 1));
      [arr[i], arr[j]] = [arr[j], arr[i]];
    }
    return arr;
  }

  const allPhotos = Array.from({length: 45}, (_, i) => ({
    src: `/photos/harvey-chu-${i + 1}.jpeg`,
    alt: `Harvey Chu Seattle Bellevue — photo ${i + 1}`
  }));

  const photos = Object.freeze(shuffle(allPhotos));

  let currentPhoto = null;
  let currentIndex = -1;
  let startX = 0;
  let dragX = 0;
  let dragging = false;
  let slideDir = 0;
  let animating = false;

  function open(i) {
    currentIndex = i;
    currentPhoto = photos[i];
    dragX = 0;
    slideDir = 0;
  }

  function close() {
    currentIndex = -1;
    currentPhoto = null;
    dragX = 0;
  }

  function go(i, dir = 0) {
    if (animating) return;
    animating = true;
    slideDir = dir;
    setTimeout(() => {
      const next = ((i % photos.length) + photos.length) % photos.length;
      currentIndex = next;
      currentPhoto = photos[next];
      dragX = 0;
      slideDir = 0;
      animating = false;
    }, 220);
  }

  function next() { go(currentIndex + 1, -1); }
  function prev() { go(currentIndex - 1, 1); }

  function touchStart(e) {
    startX = e.touches[0].clientX;
    dragX = 0;
    dragging = true;
  }

  function touchMove(e) {
    if (!dragging) return;
    dragX = e.touches[0].clientX - startX;
  }

  function touchEnd(e) {
    dragging = false;
    const diff = startX - e.changedTouches[0].clientX;
    if (Math.abs(diff) > 50) {
      diff > 0 ? next() : prev();
    } else {
      dragX = 0;
    }
  }

  function onKey(e) {
    if (!currentPhoto) return;
    if (e.key === 'ArrowRight') next();
    if (e.key === 'ArrowLeft') prev();
    if (e.key === 'Escape') close();
  }

  $: imgStyle = dragging
    ? `transform: translateX(${dragX}px); transition: none;`
    : slideDir !== 0
    ? `transform: translateX(${slideDir * 60}px); opacity: 0; transition: transform 0.22s ease, opacity 0.22s ease;`
    : `transform: translateX(0); opacity: 1; transition: transform 0.22s ease, opacity 0.22s ease;`;
</script>

<svelte:window on:keydown={onKey} />

<h1>Photos.</h1>
<p class="sub">Some memories from the past.</p>

<div class="grid">
  {#each photos as photo, i (photo.src)}
    <button class="thumb" on:click={() => open(i)} aria-label="Open photo {i + 1}">
      <img src={photo.src} alt={photo.alt} loading="lazy" />
    </button>
  {/each}
</div>

{#if currentPhoto}
  <div
    class="lightbox"
    role="dialog"
    aria-modal="true"
    aria-label="Photo lightbox"
    on:touchstart={touchStart}
    on:touchmove={touchMove}
    on:touchend={touchEnd}
  >
    <button class="lb-close" on:click={close} aria-label="Close">✕</button>

    <div class="lb-img-wrap">
      <img
        class="lb-img"
        src={currentPhoto.src}
        alt={currentPhoto.alt}
        style={imgStyle}
      />
    </div>

    <button class="lb-prev" on:click={prev} aria-label="Previous">
      <span class="arrow-circle">&#8249;</span>
    </button>
    <button class="lb-next" on:click={next} aria-label="Next">
      <span class="arrow-circle">&#8250;</span>
    </button>

    <p class="lb-counter">{currentIndex + 1} / {photos.length}</p>
  </div>
{/if}

<style>
  h1 {
    font-size: 2rem;
    font-weight: 400;
    margin-bottom: 0.75rem;
    line-height: 1.2;
  }

  .sub {
    font-size: 15px;
    color: #888;
    line-height: 1.7;
    max-width: 520px;
    margin-bottom: 1.5rem;
  }

  .grid {
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    gap: 4px;
    width: 100%;
  }

  .thumb {
    aspect-ratio: 1;
    overflow: hidden;
    background: #f0efeb;
    border: none;
    padding: 0;
    cursor: pointer;
    display: block;
  }

  .thumb img {
    width: 100%;
    height: 100%;
    object-fit: cover;
    display: block;
    transition: opacity 0.2s ease;
  }

  .thumb:hover img { opacity: 0.82; }

  /* Lightbox */
  .lightbox {
    position: fixed;
    inset: 0;
    background: rgba(0, 0, 0, 0.95);
    display: flex;
    align-items: center;
    justify-content: center;
    flex-direction: column;
    z-index: 999;
    user-select: none;
  }

  .lb-img-wrap {
    width: 88vw;
    max-width: 860px;
    display: flex;
    align-items: center;
    justify-content: center;
    overflow: hidden;
  }

  .lb-img {
    max-width: 100%;
    max-height: 78vh;
    object-fit: contain;
    border-radius: 3px;
    will-change: transform, opacity;
  }

  .lb-close {
    position: fixed;
    top: 1.25rem;
    right: 1.5rem;
    background: rgba(255, 255, 255, 0.1);
    border: none;
    color: #fff;
    font-size: 1.2rem;
    cursor: pointer;
    width: 38px;
    height: 38px;
    border-radius: 50%;
    display: flex;
    align-items: center;
    justify-content: center;
    opacity: 0.7;
    transition: background 0.2s, opacity 0.2s;
  }

  .lb-close:hover {
    background: rgba(255, 255, 255, 0.2);
    opacity: 1;
  }

  /* Arrow buttons */
  .lb-prev,
  .lb-next {
    position: fixed;
    top: 50%;
    transform: translateY(-50%);
    background: none;
    border: none;
    cursor: pointer;
    padding: 0 1rem;
    line-height: 1;
    display: flex;
    align-items: center;
    justify-content: center;
  }

  .lb-prev { left: 0.5rem; }
  .lb-next { right: 0.5rem; }

  .arrow-circle {
    display: flex;
    align-items: center;
    justify-content: center;
    width: 52px;
    height: 52px;
    border-radius: 50%;
    background: rgba(255, 255, 255, 0.12);
    color: #fff;
    font-size: 2.2rem;
    line-height: 1;
    padding-bottom: 2px;
    transition: background 0.2s, transform 0.15s;
    backdrop-filter: blur(4px);
  }

  .lb-prev:hover .arrow-circle {
    background: rgba(255, 255, 255, 0.25);
    transform: scale(1.08);
  }

  .lb-next:hover .arrow-circle {
    background: rgba(255, 255, 255, 0.25);
    transform: scale(1.08);
  }

  .lb-counter {
    color: rgba(255, 255, 255, 0.35);
    font-family: 'Courier New', monospace;
    font-size: 12px;
    margin-top: 1.2rem;
    letter-spacing: 0.06em;
  }

  /* Responsive grid */
  @media (min-width: 1200px) {
    .grid { grid-template-columns: repeat(5, 1fr); }
  }

  @media (max-width: 900px) {
    .grid { grid-template-columns: repeat(3, 1fr); }
  }

  @media (max-width: 600px) {
    .grid { grid-template-columns: repeat(3, 1fr); gap: 3px; }
    .lb-img-wrap { width: 96vw; }
    .lb-img { max-height: 75vh; }
    .arrow-circle { width: 42px; height: 42px; font-size: 1.8rem; }
    .lb-prev { left: 0.25rem; }
    .lb-next { right: 0.25rem; }
    .lb-close { top: 1rem; right: 1rem; }
  }

  @media (max-width: 380px) {
    .grid { grid-template-columns: repeat(2, 1fr); gap: 3px; }
  }
</style>