<script>
	import favicon from '$lib/assets/favicon.svg';
	import ZooBanner from "$lib/components/zoo.svelte"

	let { children } = $props();

</script>

<svelte:head>
	<link rel="icon" href={favicon} />
	<!-- Pets Icon, Google Fonts -->
	<link rel="stylesheet" href="https://fonts.googleapis.com/css2?family=Material+Symbols+Outlined:opsz,wght,FILL,GRAD@20..48,100..700,0..1,-50..200&icon_names=pets" />
</svelte:head>

<!-- Banner from #the-zoo ysws -->
<ZooBanner />

{@render children()}
