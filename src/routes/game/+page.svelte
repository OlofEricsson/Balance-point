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
	class="ml-auto mr-auto"
	width="1000"
	height="700"
	style="image-rendering: pixelated;"
></canvas>

<section style="width:60vw; margin-left: 30vw; margin-right: 30vw;">
	<section id="errorText" class="text-2xl font-bold mt-32">Game isn't supported on mobile layout</section>

	<section id="hometext" class="text-lg font-bold mt-32">
		<section class="text-2xl">Controls:</section>
		<p>⬆️ to accelerate</p>
		<p>⬇️ to brake</p>
		<p>⬅️ and ➡️ to control bike</p>
	</section>

	<section id="hometext" class="font-bold text-lg mb-32 mt-32">

		<p>Balance point is a game about wheelies. The goal is to score as high as possible. Control the bike using arrowkeys, the greater the angle and the higher the speed the more points you gain. Going too far back however results in a crash</p>

	</section>
</section>

<style>
	canvas {
		border: 2px solid rgb(255, 255, 255);
	}

	@media (max-width: 1000px) {
		canvas {
			display: none;
		}
	}

	@media (min-width: 1000px) {
		#errorText {
			display: none;
		}
	}
</style>
