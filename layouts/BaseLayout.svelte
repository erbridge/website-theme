<script lang="ts">
  import Nav from "../components/Nav.svelte";

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

  main {
    flex-grow: 1;
  }

  ul {
    margin-left: 0;
    padding-left: 0;
    display: flex;
    justify-content: space-evenly;
    list-style: none;
  }
</style>
