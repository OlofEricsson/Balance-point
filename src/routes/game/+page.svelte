<script>
	import { onMount } from 'svelte';

	onMount(() => {
		const canvas = document.querySelector('#myCanvas');
		const ctx = canvas.getContext('2d');

		ground(ctx);
		wheel(ctx);

	});

	function wheel(ctx) {
		ctx.beginPath();
		ctx.arc(200, 575, 50, 0, 2 * Math.PI);
		ctx.fillStyle = 'black'; //funkar ej
		ctx.stroke();
	}

	function ground(ctx) {
		ctx.beginPath();
		ctx.fillStyle = 'rgb(100, 100, 100)';
		ctx.fillRect(0, 500, 1000, 200);
	}

</script>

<p>hej hej hallå</p>

<canvas
	id="myCanvas"
	class="m-auto bg-blue-400"
	width="1000"
	height="700"
	style="image-rendering: pixelated;"
></canvas>

<style>
	canvas {
		border: 2px solid rgb(255, 255, 255);
	}
</style>
