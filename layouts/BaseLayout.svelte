<script lang="ts">
	import { AtSign, ChevronDown, Github, MessageSquare } from "lucide-svelte";
	import Nav from "../components/Nav.svelte";

	export let pathname: string;
	export let contactEmail: string;
	export let internalLinks: {
		href: string;
		pathPrefix: string;
		exact?: boolean;
		label: string;
	}[];
	export let externalLink:
		| { href: string; label: string; rel?: string[] }
		| undefined = undefined;
</script>

<header>
	<Nav {pathname} {internalLinks} {externalLink} />
</header>

<main>
	<slot name="main" />
</main>

<slot name="aside" />

<div class="scroll-hint">
	<ChevronDown aria-label="scroll down" />
</div>

<footer>
	<ul>
		<li>
			<MessageSquare role="presentation" />
			<a rel="external me" href="https://sunspot.garden/@erbridge"> social</a>
		</li>
		<li>
			<Github role="presentation" />
			<a rel="external me" href="https://github.com/erbridge"> code</a>
		</li>
		<li>
			<AtSign role="presentation" />
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

	main {
		min-height: 0;
		flex-grow: 1;
	}

	.scroll-hint {
		position: sticky;
		margin: 0 auto;
		padding-top: 2rem;
		width: fit-content;
		bottom: 0;
		z-index: -1;
	}

	.scroll-hint :global(svg) {
		width: 3em;
		height: 3em;
		color: rgba(var(--text-colour), 0.5);
	}

	footer {
		max-width: 100%;
		width: 100%;
		position: absolute;
		bottom: 0;
		background-color: rgb(var(--background-colour));
		box-shadow: 0px -10px 10px 0px rgb(var(--background-colour));
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
</style>
