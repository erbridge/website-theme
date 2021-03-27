<script lang="ts">
  import feather from "feather-icons";
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
      {@html feather.icons.twitter.toSvg({ role: "presentation" })}
      <a rel="external me" href="https://twitter.com/erbridge"> twitter</a>
    </li>
    <li>
      {@html feather.icons.github.toSvg({ role: "presentation" })}
      <a rel="external me" href="https://github.com/erbridge"> github</a>
    </li>
    <li>
      {@html feather.icons["at-sign"].toSvg({ role: "presentation" })}
      <a href="mailto:{contactEmail}"> email</a>
    </li>
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

  ul :global(svg) {
    width: 1em;
    height: 1em;
    vertical-align: bottom;
    color: rgb(var(--accent-colour));
  }

  @media (min-width: 768px) {
    header > div {
      margin-top: 0.3rem;
      right: 1rem;
      left: auto;
    }
  }
</style>
