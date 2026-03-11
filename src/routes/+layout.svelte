<script>
	import './layout.css';
	import favicon from '$lib/assets/favicon.svg';

	let { children } = $props();
</script>

<svelte:head><link rel="icon" href={favicon} /></svelte:head>

<div data-sveltekit-preload-data="hover" data-sveltekit-preload-code="viewport" class="flex flex-row ">
	<div class="text-white text-center font-bold fontsans text-4xl p-5 absolute left-1/2 -translate-x-1/2">
		<a href="/home">Balance point</a>
	</div>
	<div class="text-white text-right font-bold fontsans text-4xl p-5 place-self-end ml-auto">
		<a href="/game">game</a>
	</div>
</div>

<div class="grid grid-cols-[1fr_60vw_1fr] h-screen">
	<div class="col-start-2 col-end-2 text-white">

		{@render children()}

	</div>

</div>