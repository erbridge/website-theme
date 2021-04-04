<script lang="ts">
  import ThemeToggle from "./ThemeToggle.svelte";

  export let segment: string;
  export let internalLinks: {
    href: string;
    segment: string | undefined;
    label?: string;
  }[];
  export let externalLink: { href: string; label: string; rel?: string[] };
</script>

<nav>
  <ul>
    {#each internalLinks as link}
      <li>
        <a
          sapper:prefetch
          sapper:noscroll
          aria-current={segment === link.segment ? "page" : undefined}
          href={link.href}>{link.label || link.href}</a
        >
      </li>
    {/each}

    <li>
      <a
        rel="external{externalLink.rel ? ` ${externalLink.rel.join(' ')}` : ''}"
        href={externalLink.href}>{externalLink.label}</a
      >
    </li>
  </ul>

  <ThemeToggle />
</nav>

<style>
  nav {
    font-family: var(--heading-font-family);
    font-weight: 600;
    font-size: 1.2rem;
    display: flex;
    align-items: center;
  }

  ul {
    flex: 1;
    margin: 0;
    padding: 0;
    display: flex;
    flex-wrap: wrap;
    line-height: 2;
    list-style: none;
  }

  li {
    white-space: nowrap;
  }

  li:last-child {
    margin-left: auto;
  }

  li:last-child a {
    margin-left: 2em;
  }

  a {
    border-bottom: 2px double rgb(var(--text-colour));
    padding: 0.1em 0.2em;
    text-decoration: none;
  }

  a:active,
  a:focus,
  a:focus-within,
  a:hover {
    border-bottom-color: rgb(var(--accent-colour));
  }

  a[href^="http"]:active,
  a[href^="http"]:focus,
  a[href^="http"]:focus-within,
  a[href^="http"]:hover {
    border-bottom-color: rgb(var(--secondary-accent-colour));
  }

  a[href^="http"]::after {
    content: "";
  }

  a:not(:last-child) {
    margin-right: 0.5em;
  }

  [aria-current] {
    border-bottom: 2px solid rgb(var(--accent-colour));
  }
</style>
