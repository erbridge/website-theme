<script lang="ts">
	import { onMount } from "svelte";

	export let value: string;

	const date = new Date(value);

	let formatter: Intl.DateTimeFormat;

	function setFormatter() {
		formatter = new Intl.DateTimeFormat("en-GB", {
			calendar: "gregory",
			dateStyle: "long",
		} as Intl.DateTimeFormatOptions);
	}

	if ("DateTimeFormat" in Intl) {
		setFormatter();
	}

	onMount(async () => {
		await Promise.all([
			import("@formatjs/intl-getcanonicallocales/polyfill"),
			import("@formatjs/intl-locale/polyfill"),
			import("@formatjs/intl-pluralrules/polyfill"),
			import("@formatjs/intl-numberformat/polyfill"),
			import("@formatjs/intl-numberformat/locale-data/en-GB"),
			import("@formatjs/intl-datetimeformat/polyfill"),
			import("@formatjs/intl-datetimeformat/locale-data/en-GB"),
		]);

		setFormatter();
	});
</script>

<time datetime={date.toISOString().split("T")[0]}
	>{formatter ? formatter.format(date) : date.toLocaleDateString()}</time
>
