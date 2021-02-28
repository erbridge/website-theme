<script lang="ts">
  import Nav from "../components/Nav.svelte";
  import ThemeToggle from "../components/ThemeToggle.svelte";

  export let segment: string;
  export let contactEmail: string;
  export let internalLinks: {
    href: string;
    segment: string | undefined;
    label?: string;
  }[];
  export let externalLink: { href: string; label: string; rel?: string[] };
</script>

<header>
  <Nav {segment} {internalLinks} {externalLink} />

  <div><ThemeToggle /></div>
</header>

<main>
  <slot name="main" />
</main>

<slot name="aside" />

<footer>
  <ul>
    <li>
      <a rel="external me" href="https://twitter.com/erbridge">twitter</a>
    </li>
    <li><a rel="external me" href="https://github.com/erbridge">github</a></li>
    <li><a href="mailto:{contactEmail}">email</a></li>
  </ul>
</footer>

<style>
  header,
  main,
  footer {
    max-width: 58rem;
    width: 100%;
    margin: 0 auto;
    padding: 1rem;
  }

  header {
    position: relative;
  }

  header > div {
    margin-top: 0.2rem;
    position: absolute;
    left: 1rem;
    z-index: 1;
    font-size: 0.8em;
  }

  main {
    min-height: 0;
    flex-grow: 1;
  }

  ul {
    margin-left: 0;
    padding-left: 0;
    display: flex;
    justify-content: space-evenly;
    list-style: none;
  }

  @media (min-width: 700px) {
    header > div {
      margin-top: 0.3rem;
      right: 1rem;
      left: auto;
    }
  }
</style>
