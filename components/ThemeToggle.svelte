<script lang="ts">
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

  function endThemeTrial(event?: Event) {
    if (event?.target === document.activeElement) {
      return;
    }

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
    on:click={toggleTheme}>go {theme === "light" ? "dark" : "light"}</button
  >
{/if}
