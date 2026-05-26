<script>
  function shuffle(arr) {
    for (let i = arr.length - 1; i > 0; i--) {
      const j = Math.floor(Math.random() * (i + 1));
      [arr[i], arr[j]] = [arr[j], arr[i]];
    }
    return arr;
  }

  const photos = shuffle(Array.from({length: 45}, (_, i) => ({
    src: `/photos/harvey-chu-${i + 1}.jpeg`,
    alt: `Harvey Chu Seattle Bellevue — photo ${i + 1}`
  })));

  let current = -1;
  let startX = 0;

  function open(i) { current = i; }
  function close() { current = -1; }
  function go(i) { current = ((i % photos.length) + photos.length) % photos.length; }
  function next() { go(current + 1); }
  function prev() { go(current - 1); }

  function touchStart(e) { startX = e.touches[0].clientX; }
  function touchEnd(e) {
    const diff = startX - e.changedTouches[0].clientX;
    if (Math.abs(diff) > 40) diff > 0 ? next() : prev();
  }

  function onKey(e) {
    if (current < 0) return;
    if (e.key === 'ArrowRight') next();
    if (e.key === 'ArrowLeft') prev();
    if (e.key === 'Escape') close();
  }
</script>

<svelte:window on:keydown={onKey} />

<h1>Photos.</h1>
<p class="sub">A few photos from Seattle and beyond.</p>

<div class="grid">
  {#each photos as photo, i}
    <button class="thumb" on:click={() => open(i)} aria-label="Open photo {i + 1}">
      <img src={photo.src} alt={photo.alt} loading="lazy" />
    </button>
  {/each}
</div>

{#if current >= 0}
  <div
    class="lightbox"
    role="dialog"
    aria-modal="true"
    aria-label="Photo lightbox"
    on:touchstart={touchStart}
    on:touchend={touchEnd}
  >
    <button class="lb-close" on:click={close} aria-label="Close">✕</button>
    <button class="lb-prev" on:click={prev} aria-label="Previous">&#8249;</button>
    <img class="lb-img" src={photos[current].src} alt={photos[current].alt} />
    <button class="lb-next" on:click={next} aria-label="Next">&#8250;</button>
    <p class="lb-counter">{current + 1} / {photos.length}</p>
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

  /* Grid — responsive columns based on screen size */
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

  .thumb:hover img {
    opacity: 0.82;
  }

  /* Lightbox */
  .lightbox {
    position: fixed;
    inset: 0;
    background: rgba(0, 0, 0, 0.93);
    display: flex;
    align-items: center;
    justify-content: center;
    flex-direction: column;
    z-index: 999;
  }

  .lb-img {
    max-width: 88vw;
    max-height: 78vh;
    object-fit: contain;
    border-radius: 3px;
  }

  .lb-close {
    position: fixed;
    top: 1.25rem;
    right: 1.5rem;
    background: none;
    border: none;
    color: #fff;
    font-size: 1.4rem;
    cursor: pointer;
    opacity: 0.6;
    line-height: 1;
    padding: 6px 10px;
  }

  .lb-close:hover { opacity: 1; }

  .lb-prev,
  .lb-next {
    position: fixed;
    top: 50%;
    transform: translateY(-50%);
    background: none;
    border: none;
    color: #fff;
    font-size: 3rem;
    cursor: pointer;
    opacity: 0.45;
    padding: 0 1.25rem;
    line-height: 1;
  }

  .lb-prev { left: 0; }
  .lb-next { right: 0; }
  .lb-prev:hover,
  .lb-next:hover { opacity: 1; }

  .lb-counter {
    color: rgba(255, 255, 255, 0.35);
    font-family: 'Courier New', monospace;
    font-size: 12px;
    margin-top: 1rem;
    letter-spacing: 0.06em;
  }

  /* Large desktop — 5 columns */
  @media (min-width: 1200px) {
    .grid {
      grid-template-columns: repeat(5, 1fr);
    }
  }

  /* Tablet landscape / small desktop — 4 columns (default above) */

  /* Tablet portrait — 3 columns */
  @media (max-width: 900px) {
    .grid {
      grid-template-columns: repeat(3, 1fr);
    }
  }

  /* Large phone — 3 columns */
  @media (max-width: 600px) {
    .grid {
      grid-template-columns: repeat(3, 1fr);
      gap: 3px;
    }

    .lb-img {
      max-width: 96vw;
      max-height: 75vh;
    }

    .lb-prev,
    .lb-next {
      font-size: 2.2rem;
      padding: 0 0.6rem;
    }

    .lb-close {
      top: 1rem;
      right: 1rem;
    }
  }

  /* Small phone — 2 columns */
  @media (max-width: 380px) {
    .grid {
      grid-template-columns: repeat(2, 1fr);
      gap: 3px;
    }
  }
</style>