<script>
	import favicon from "$lib/assets/favicon.svg";
	import ZooBanner from "$lib/components/zoo.svelte";
	import Footer from "$lib/components/footer.svelte";
	import '../app.css'

	let { children } = $props();
</script>

<svelte:head>
	<link rel="icon" href={favicon} />
	<!-- Pets, Destruction, Skull, Rowing Icon, Google Fonts (Broooo it had to be alphabetically ordered)-->
	
	<link rel="stylesheet" href="https://fonts.googleapis.com/css2?family=Material+Symbols+Outlined:opsz,wght,FILL,GRAD@20..48,100..700,0..1,-50..200&icon_names=destruction,pets,rowing,skull" /> 
	

	
</svelte:head>

<!-- Banner from #the-zoo ysws -->
<ZooBanner />

{@render children()}

<Footer />

