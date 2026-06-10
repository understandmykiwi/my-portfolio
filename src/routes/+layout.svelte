<script>
  import { page } from '$app/stores';

  $: showHero = $page.url.pathname === '/' || $page.url.pathname === '/about';
</script>

<nav>
  <a href="/" class="logo">Harvey Chu</a>
  <div class="nav-links">
    <a href="/" class:active={$page.url.pathname === '/'}>Home</a>
    <a href="/about" class:active={$page.url.pathname === '/about'}>About</a>
    <a href="/photos" class:active={$page.url.pathname === '/photos'}>Photos</a>
  </div>
</nav>

{#if showHero}
  <div class="hero">
    <img src="/harvey-chu-hero.jpeg" alt="Harvey Chu Seattle Bellevue compliance executive" />
  </div>
{/if}

<main>
  <slot />
</main>

<footer>
  <span>© 2026 Harvey Chu</span>
  <div class="footer-links">
    <a href="/linkedin" target="_blank" rel="noopener noreferrer">LinkedIn</a>
    <a href="mailto:hello@harveychu.com">Email</a>
  </div>
</footer>

<style>
  :global(*, *::before, *::after) { margin: 0; padding: 0; box-sizing: border-box; }
  :global(body) {
    font-family: Georgia, 'Times New Roman', serif;
    font-size: 17px;
    line-height: 1.7;
    background: #fff;
    color: #111;
  }

  nav {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 2rem 2rem;
    max-width: 800px;
    margin: 0 auto;
    width: 100%;
  }

  /* Border below nav matching Daniel's site */
  nav {
    border-bottom: 1px solid #e5e5e5;
    max-width: 100%;
    padding: 2rem 3rem;
  }

  .logo {
    font-family: 'Courier New', monospace;
    font-weight: 700;
    font-size: 17px;
    letter-spacing: 0.08em;
    text-transform: uppercase;
    text-decoration: none;
    color: #111;
  }

  .nav-links {
    display: flex;
    gap: 1.5rem;
  }

  .nav-links a {
    font-family: Georgia, serif;
    font-size: 17px;
    text-decoration: none;
    color: #aaa;
  }

  .nav-links a:hover,
  .nav-links a.active {
    color: #111;
  }

  /* Hero photo */
  .hero {
    width: 100%;
    max-width: 800px;
    margin: 0 auto;
    overflow: hidden;
    border-radius: 6px;
    padding: 2rem 2rem 0;
  }

  .hero img {
    width: 100%;
    height: 320px;
    object-fit: cover;
    object-position: center top;
    display: block;
    border-radius: 6px;
  }

  main {
    max-width: 800px;
    margin: 0 auto;
    padding: 2.5rem 2rem 5rem;
    width: 100%;
  }

  footer {
    max-width: 100%;
    border-top: 1px solid #e5e5e5;
    margin: 0 auto;
    padding: 1.5rem 3rem;
    display: flex;
    justify-content: space-between;
    align-items: center;
    font-size: 13px;
    color: #bbb;
    width: 100%;
  }

  .footer-links {
    display: flex;
    gap: 1.5rem;
  }

  footer a {
    color: #bbb;
    text-decoration: none;
  }

  footer a:hover {
    color: #888;
  }

  /* Tablet */
  @media (max-width: 900px) {
    .hero img { height: 260px; }
    nav { padding: 1.8rem 2rem; }
    footer { padding: 1.5rem 2rem; }
  }

  /* Large phone */
  @media (max-width: 600px) {
    nav { padding: 1.4rem 1.25rem; }
    .nav-links { gap: 1rem; }
    .nav-links a { font-size: 15px; }
    .hero { padding: 1.25rem 1.25rem 0; }
    .hero img { height: 200px; object-position: center 20%; }
    main { padding: 2rem 1.25rem 4rem; }
    footer { padding: 1.25rem; flex-direction: column; align-items: flex-start; gap: 0.5rem; }
  }

  /* Small phone */
  @media (max-width: 380px) {
    .hero img { height: 160px; }
  }
</style>