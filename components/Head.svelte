<script lang="ts">
	export let type = "website";

	export let description: string | null = null;
	export let image: string | null = null;
	export let title: string | null = null;
</script>

<svelte:head>
	{#if title}
		<title>{title} - erbridge</title>
		<meta property="og:title" content={title} />
	{:else}
		<title>erbridge</title>
	{/if}

	{#if description || title}
		<meta name="description" content={description || title} />
		<meta property="og:description" content={description || title} />
	{/if}

	<meta property="og:type" content={type} />

	{#if image}
		<meta property="og:image" content={image} />
	{/if}

	<meta property="og:locale" content="en_GB" />
	<meta property="og:site_name" content="erbridge" />

	<slot />
</svelte:head>
