<script lang="ts">
  import { Moon, Sun } from "@erbridge/svelte-feather";
  import { onMount } from "svelte";

  let theme: "dark" | "light" | null = null;
  let trialTheme: typeof theme = null;

  function toggleTheme() {
    if (!theme) {
      return;
    }

    theme = theme === "dark" ? "light" : "dark";

    endThemeTrial();
  }

  function startThemeTrial() {
    trialTheme = theme === "dark" ? "light" : "dark";

    document.documentElement.classList.add(
      trialTheme === "dark" ? "theme--dark" : "theme--light"
    );
    document.documentElement.classList.remove(
      trialTheme === "dark" ? "theme--light" : "theme--dark"
    );
  }

  function endThemeTrial() {
    document.documentElement.classList.add(
      theme === "dark" ? "theme--dark" : "theme--light"
    );
    document.documentElement.classList.remove(
      theme === "dark" ? "theme--light" : "theme--dark"
    );

    trialTheme = null;
  }

  onMount(() => {
    if (window.matchMedia) {
      function initThemeToggle(value: "dark" | "light") {
        document.documentElement.classList.add(
          value === "dark" ? "theme--dark" : "theme--light"
        );
        document.documentElement.classList.remove(
          value === "dark" ? "theme--light" : "theme--dark"
        );

        theme = value;
        trialTheme = null;
      }

      window
        .matchMedia("(prefers-color-scheme: light)")
        .addEventListener("change", (event) => {
          initThemeToggle(event.matches ? "light" : "dark");
        });

      initThemeToggle(
        window.matchMedia("(prefers-color-scheme: light)").matches
          ? "light"
          : "dark"
      );
    }
  });
</script>

{#if theme}
  <button
    on:focus={startThemeTrial}
    on:blur={endThemeTrial}
    on:mouseenter={startThemeTrial}
    on:mouseleave={endThemeTrial}
    on:click={toggleTheme}
  >
    {#if theme === "dark"}
      <Sun fill="currentColor" aria-label="change to light theme" />
    {:else}
      <Moon fill="currentColor" aria-label="change to dark theme" />
    {/if}
  </button>
{/if}

<style>
  button {
    margin-left: 0.5em;
    border: none;
    padding: 0;
  }

  button:active,
  button:focus,
  button:focus-within,
  button:hover {
    color: rgb(var(--accent-colour));
    background: none;
  }

  button :global(*) {
    pointer-events: none;
  }

  button :global(svg) {
    display: block;
    width: 1em;
    height: 1em;
  }
</style>
