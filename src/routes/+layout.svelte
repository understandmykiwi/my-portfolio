<script>
  import { page } from '$app/stores';

  $: showHero = $page.url.pathname === '/' || $page.url.pathname === '/about';
</script>

<nav>
  <div class="nav-inner">
    <a href="/" class="logo">Harvey Chu</a>
    <div class="nav-links">
      <a href="/" class:active={$page.url.pathname === '/'}>Home</a>
      <a href="/about" class:active={$page.url.pathname === '/about'}>About</a>
      <a href="/photos" class:active={$page.url.pathname === '/photos'}>Photos</a>
    </div>
  </div>
</nav>

{#if showHero}
  <div class="hero-wrap">
    <div class="hero">
      <img src="/harvey-chu-hero.jpeg" alt="Harvey Chu Seattle Bellevue" />
    </div>
  </div>
{/if}

<main>
  <slot />
</main>

<footer>
  <div class="footer-inner">
    <span>© 2026 Harvey Chu</span>
    <div class="footer-links">
      <a href="/linkedin" target="_blank" rel="noopener noreferrer">LinkedIn</a>
      <a href="mailto:hello@harveychu.com">Email</a>
    </div>
  </div>
</footer>

<style>
  :global(*, *::before, *::after) { margin: 0; padding: 0; box-sizing: border-box; }
  :global(body) {
    font-family: Georgia, 'Times New Roman', serif;
    font-size: 17px;
    line-height: 1.7;
    background: #fff;
    color: #0f172a;
  }

  /* Full-width border, inner content centered */
  nav {
    border-bottom: 1px solid #e2e8f0;
    width: 100%;
  }

  .nav-inner {
    max-width: 768px;
    margin: 0 auto;
    padding: 1.25rem 1.5rem;
    display: flex;
    justify-content: space-between;
    align-items: center;
  }

  .logo {
    font-family: 'Courier New', monospace;
    font-weight: 700;
    font-size: 15px;
    letter-spacing: 0.08em;
    text-transform: uppercase;
    text-decoration: none;
    color: #0f172a;
  }

  .nav-links {
    display: flex;
    gap: 1.5rem;
  }

  .nav-links a {
    font-family: Georgia, serif;
    font-size: 15px;
    text-decoration: none;
    color: #64748b;
    transition: color 0.15s;
  }

  .nav-links a:hover,
  .nav-links a.active {
    color: #0f172a;
  }

  /* Hero */
  .hero-wrap {
    max-width: 768px;
    margin: 0 auto;
    padding: 2rem 1.5rem 0;
  }

  .hero {
    overflow: hidden;
    border-radius: 8px;
  }

  .hero img {
    width: 100%;
    height: 340px;
    object-fit: cover;
    object-position: center top;
    display: block;
    border-radius: 8px;
  }

  main {
    max-width: 768px;
    margin: 0 auto;
    padding: 3rem 1.5rem 6rem;
    width: 100%;
  }

  footer {
    border-top: 1px solid #e2e8f0;
    width: 100%;
  }

  .footer-inner {
    max-width: 768px;
    margin: 0 auto;
    padding: 2rem 1.5rem;
    display: flex;
    justify-content: space-between;
    align-items: center;
    font-size: 13px;
    color: #94a3b8;
  }

  .footer-links {
    display: flex;
    gap: 1.5rem;
  }

  footer a {
    color: #94a3b8;
    text-decoration: none;
    transition: color 0.15s;
  }

  footer a:hover {
    color: #0f172a;
  }

  /* Tablet */
  @media (max-width: 900px) {
    .hero img { height: 280px; }
  }

  /* Large phone */
  @media (max-width: 600px) {
    .nav-inner { padding: 1rem 1.25rem; }
    .nav-links { gap: 1rem; }
    .nav-links a { font-size: 14px; }
    .hero-wrap { padding: 1.25rem 1.25rem 0; }
    .hero img { height: 200px; object-position: center 20%; }
    main { padding: 2rem 1.25rem 4rem; }
    .footer-inner { padding: 1.25rem; flex-direction: column; align-items: flex-start; gap: 0.5rem; }
  }

  /* Small phone */
  @media (max-width: 380px) {
    .hero img { height: 160px; }
  }
</style>