<script>
	import './layout.css';
	import favicon from '$lib/assets/favicon.svg';

	let { children } = $props();
</script>

<svelte:head><link rel="icon" href={favicon} /></svelte:head>

<h1 class="text-white text-center font-bold fontsans text-4xl p-5">Balance point</h1>

<div class="grid grid-cols-[1fr_60vw_1fr] 
		h-screen">
	<div class="col-start-2 col-end-2 text-white">

		{@render children()}

	</div>

</div>