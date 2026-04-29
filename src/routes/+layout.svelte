<script>
	import './layout.css';
	import favicon from '$lib/assets/favicon.svg';
	import { page } from '$app/stores';

	let { children } = $props();
</script>

<svelte:head><link rel="icon" href={favicon} /></svelte:head>

<div data-sveltekit-preload-data="hover" data-sveltekit-preload-code="viewport" class="flex flex-row ">
	<div class="text-white text-center font-bold fontsans text-4xl p-5 absolute left-1/2 -translate-x-1/2">
		<a href="/home">Balance point</a>
	</div>
	<div class="text-white text-right font-bold fontsans text-4xl p-5 place-self-end ml-auto">
		<a href="/game">Game</a>
	</div>
</div>

<div
	id="children"
	class="grid h-screen"
	class:grid-cols-[auto_minmax(60vw,60vw)_auto]={$page.url.pathname === '/home'}
	class:grid-cols-[auto_minmax(1000px,60vw)_auto]={$page.url.pathname !== '/home'}
>
	<div class="col-start-2 col-end-2 text-white">

		{@render children()}

	</div>

</div>

<style>

	@media (max-width: 1600px) {
		#children {
			
		}
	}
</style>