<script>
  // Add your photo filenames to the static/photos/ folder and list them here
  const photos = [
    '2.PNG',
    '/photos/photo2.jpg',
    '/photos/photo3.jpg',
    '/photos/photo4.jpg',
    '/photos/photo5.jpg',
  ];

  let current = 0;
  let startX = 0;

  function next() { current = (current + 1) % photos.length; }
  function prev() { current = (current - 1 + photos.length) % photos.length; }
  function goTo(i) { current = i; }
  function pad(n) { return String(n).padStart(2, '0'); }

  function touchStart(e) { startX = e.touches[0].clientX; }
  function touchEnd(e) {
    const diff = startX - e.changedTouches[0].clientX;
    if (Math.abs(diff) > 40) diff > 0 ? next() : prev();
  }
</script>

<h1>Photos.</h1>
<p class="sub">A few photos from Seattle and beyond.</p>

<div class="gallery">
  <div class="track" on:touchstart={touchStart} on:touchend={touchEnd}>
    <img src={photos[current]} alt="Photo {current + 1}" />
  </div>

  <div class="controls">
    <button on:click={prev}>← prev</button>
    <button on:click={next}>next →</button>
    <span class="count">{pad(current + 1)} / {pad(photos.length)}</span>
    <div class="dots">
      {#each photos as _, i}
        <button
          class="dot"
          class:active={i === current}
          on:click={() => goTo(i)}
          aria-label="Photo {i + 1}"
        />
      {/each}
    </div>
  </div>
</div>

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
    max-width: 420px;
    margin-bottom: 1.5rem;
  }
  .gallery {
    max-width: 600px;
  }
  .track {
    border-radius: 3px;
    overflow: hidden;
    background: #f5f5f5;
  }
  .track img {
    width: 100%;
    display: block;
    object-fit: cover;
    max-height: 480px;
  }
  .controls {
    display: flex;
    align-items: center;
    gap: 1.2rem;
    margin-top: 1rem;
  }
  .controls button {
    background: none;
    border: none;
    font-family: 'Courier New', monospace;
    font-size: 13px;
    color: #888;
    cursor: pointer;
    letter-spacing: 0.06em;
    padding: 0;
  }
  .controls button:hover {
    color: #111;
  }
  .count {
    font-family: 'Courier New', monospace;
    font-size: 13px;
    color: #bbb;
  }
  .dots {
    display: flex;
    gap: 5px;
    margin-left: auto;
  }
  .dot {
    width: 5px;
    height: 5px;
    border-radius: 50%;
    background: #ddd;
    border: none;
    padding: 0;
    cursor: pointer;
  }
  .dot.active {
    background: #888;
  }
</style>