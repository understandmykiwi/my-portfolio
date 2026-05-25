<script>
  import { page } from '$app/stores';
</script>

<nav>
  <a href="/" class="logo">Harvey Chu</a>
  <div class="nav-links">
    <a href="/" class:active={$page.url.pathname === '/'}>Home</a>
    <a href="/about" class:active={$page.url.pathname === '/about'}>About</a>
    <a href="/work" class:active={$page.url.pathname === '/work'}>Work</a>
    <a href="/photos" class:active={$page.url.pathname === '/photos'}>Photos</a>
  </div>
</nav>

<main>
  <slot />
</main>

<footer>
  <span>© 2026 Harvey Chu</span>
  <div class="footer-links">
    <a href="https://www.linkedin.com/in/harveychu/" target="_blank">LinkedIn</a>
    <a href="mailto:hello@harveychu.com">Email</a>
  </div>
</footer>

<style>
  :global(*, *::before, *::after) { margin: 0; padding: 0; box-sizing: border-box; }
  :global(body) {
    font-family: Georgia, 'Times New Roman', serif;
    font-size: 15px;
    line-height: 1.7;
    background: #fff;
    color: #111;
  }

  nav {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 1.5rem 2.5rem;
  }
  .logo {
    font-family: 'Courier New', monospace;
    font-weight: 700;
    font-size: 14px;
    letter-spacing: 0.08em;
    text-transform: uppercase;
    text-decoration: none;
    color: #111;
  }
  .nav-links {
    display: flex;
    gap: 2rem;
  }
  .nav-links a {
    font-family: Georgia, serif;
    font-size: 15px;
    text-decoration: none;
    color: #888;
  }
  .nav-links a:hover,
  .nav-links a.active {
    color: #111;
  }

  main {
    max-width: 760px;
    padding: 3rem 2.5rem 5rem;
  }

  footer {
    padding: 1.5rem 2.5rem;
    display: flex;
    justify-content: space-between;
    align-items: center;
    font-size: 13px;
    color: #bbb;
    margin-top: 2rem;
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
</style>