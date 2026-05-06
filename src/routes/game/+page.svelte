<script>
	import { onMount } from 'svelte';
	import {startGame} from "$lib/game.js";

	onMount(() => {
		function checkWidth() {
			if (window.innerWidth >= 800) {
				startGame();
			}
		}

		checkWidth();

		window.addEventListener('resize', checkWidth);

		return () => {
			window.removeEventListener('resize', checkWidth);
		};
	});
	
</script>

<canvas
	id="myCanvas"
	class="ml-auto mr-auto mb-5"
	width="1000"
	height="700"
	style="image-rendering: pixelated;"
></canvas>

<div class="max-w-[60vw] mx-auto">
	<div id="errorText" class="text-2xl font-bold mt-32">Game isn't supported on mobile layout</div>
	<a id="restart" href="/game" data-sveltekit-reload class="p-2 border-2 border-solid rounded-lg">Restart!</a>

	<div class="text-lg font-bold mt-32">
		<div class="text-2xl">Controls:</div>
		<p>⬆️ to accelerate</p>
		<p>⬇️ to brake</p>
		<p>⬅️ and ➡️ to control bike</p>
	</div>

	<div class="font-bold text-lg mb-32 mt-5">

		<p>Balance point is a game about wheelies. The goal is to score as high as possible. Control the bike using arrowkeys, the greater the angle and the higher the speed the more points you gain. Going too far back however results in a crash</p>

	</div>
</div>

<style>
	canvas {
		border: 2px solid rgb(255, 255, 255);
	}

	@media (max-width: 1000px) {
		canvas {
			display: none;
		}
		#restart {
			display: none;
		}
	}

	@media (min-width: 1000px) {
		#errorText {
			display: none;
		}
	}
</style>
