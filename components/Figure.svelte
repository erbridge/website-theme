<script lang="ts">
  import type { FocusTrap } from "focus-trap";
  import { onMount } from "svelte";
  import { portal } from "svelte-portal";

  export let src: string;
  export let alt: string;
  export let rootElementId = "sapper";
  export let clickable = true;

  let isMounted = false;
  let createDialogFocusTrap: (element: HTMLElement) => FocusTrap;

  onMount(async () => {
    isMounted = true;

    if (clickable) {
      const { createFocusTrap } = await import("focus-trap");

      createDialogFocusTrap = (element) =>
        createFocusTrap(element, { escapeDeactivates: false });
    }
  });

  const captionId = `${src.replace(
    new RegExp("[/.]", "g"),
    "-"
  )}-caption-${Math.floor(Math.random() * 10000)}`;

  let anchor: HTMLAnchorElement;
  let isLightboxVisible = false;

  $: if (isMounted) {
    isLightboxVisible
      ? document.body.classList.add("no-scroll")
      : document.body.classList.remove("no-scroll");
  }

  function toggleLightbox(event: MouseEvent) {
    event.preventDefault();

    isLightboxVisible = !isLightboxVisible;
  }

  function escapeLightbox(event: KeyboardEvent) {
    if (isLightboxVisible && event.key === "Escape") {
      event.preventDefault();

      isLightboxVisible = false;
    }
  }

  function onCreateLightbox(dialog: HTMLElement) {
    const root = document.getElementById(rootElementId);
    const dialogFocusTrap = createDialogFocusTrap(dialog);

    dialogFocusTrap.activate();
    root.setAttribute("aria-hidden", "true");

    return {
      destroy() {
        root.removeAttribute("aria-hidden");
        dialogFocusTrap.deactivate();
      },
    };
  }
</script>

<svelte:window on:keydown={clickable && escapeLightbox} />

<figure>
  {#if clickable}
    <a href={src} on:click={toggleLightbox} bind:this={anchor}
      ><img {src} {alt} aria-describedby={captionId} loading="lazy" /></a
    >
  {:else}
    <img {src} {alt} aria-describedby={captionId} loading="lazy" />
  {/if}
  <figcaption id={captionId}>
    <slot />
  </figcaption>
</figure>

{#if isLightboxVisible}
  <aside
    role="dialog"
    aria-label="image lightbox"
    aria-describedby="lightbox-{captionId}"
    aria-modal="true"
    use:portal={"body"}
    use:onCreateLightbox
    hidden
  >
    <div
      tabindex="0"
      role="button"
      aria-label="close lightbox"
      on:click={toggleLightbox}
    />
    <figure>
      <img {src} {alt} aria-describedby="lightbox-{captionId}" />
      <figcaption id="lightbox-{captionId}">
        <slot /><br />
        (click outside the image or press <kbd>esc</kbd> to close)
      </figcaption>
    </figure>
  </aside>
{/if}

<style>
  a:focus {
    outline: none;
  }

  a:focus img {
    border-style: dashed;
    opacity: 0.5;
  }

  [role="dialog"] {
    padding: 2rem;
    position: fixed;
    top: 0;
    left: 0;
    bottom: 0;
    right: 0;
    display: flex;
    justify-content: center;
    align-items: center;
    overflow: auto;
    background-color: rgb(var(--background-colour), 0.9);
  }

  [role="dialog"] [role="button"] {
    position: fixed;
    width: 100%;
    height: 100%;
    z-index: -1;
    cursor: pointer;
  }

  [role="dialog"] figure {
    max-height: 100%;
    margin: 0;
  }

  [role="dialog"] img {
    border: none;
  }

  :global(body.no-scroll) {
    overflow: hidden !important;
  }
</style>
