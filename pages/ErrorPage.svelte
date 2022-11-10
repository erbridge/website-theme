<script lang="ts">
	export let status: number;
	export let error: { message: string } | null;
</script>

<svelte:head>
	{#if error}
		<title>{status}: {error.message} - erbridge</title>
	{:else}
		<title>{status} - erbridge</title>
	{/if}
</svelte:head>

<h1>{status}</h1>

{#if error}
	<p>{error.message}</p>
{/if}
