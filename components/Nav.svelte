<script lang="ts">
	import ThemeToggle from "./ThemeToggle.svelte";

	export let pathname: string;
	export let internalLinks: {
		href: string;
		pathPrefix: string;
		exact?: boolean;
		label: string;
	}[];
	export let externalLink:
		| { href: string; label: string; rel?: string[] }
		| undefined;
</script>

<nav>
	<ul>
		{#each internalLinks as link}
			<li>
				<a
					data-sveltekit-noscroll
					aria-current={(
						link.exact
							? pathname === link.pathPrefix
							: pathname.startsWith(link.pathPrefix)
					)
						? "page"
						: undefined}
					href={link.href}>{link.label}</a
				>
			</li>
		{/each}

		{#if externalLink}
			<li class="left-spaced">
				<a
					rel="external{externalLink.rel
						? ` ${externalLink.rel.join(' ')}`
						: ''}"
					href={externalLink.href}>{externalLink.label}</a
				>
			</li>
		{/if}
	</ul>

	<ThemeToggle />
</nav>

<style>
	nav {
		font-family: var(--heading-font-family);
		font-weight: var(--heading-font-weight);
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

	li.left-spaced {
		margin-left: auto;
	}

	li.left-spaced a {
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
