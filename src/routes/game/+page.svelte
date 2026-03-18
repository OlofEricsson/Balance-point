<script>
	import { onMount } from 'svelte';

	const keys = {
		ArrowLeft: false,
		ArrowRight: false,
		ArrowUp: false,
		ArrowDown: false
	};

	let ctx;
	let angle = 0;
	let wheelAngle = 0;
	let angularVelocity = 0;
	let x = 200;
	let y = 575;

	const acceleration = 0.005;
	const friction = 0.98;

	onMount(() => {
		const canvas = document.querySelector('#myCanvas');
		ctx = canvas.getContext('2d');

		const handleKeyDown = (event) => {
			if (event.key in keys) {
				keys[event.key] = true;
			}
		};

		const handleKeyUp = (event) => {
			if (event.key in keys) {
				keys[event.key] = false;
			}
		};

		document.addEventListener("keydown", handleKeyDown);
		document.addEventListener("keyup", handleKeyUp);

		update();

		return () => {
			document.removeEventListener("keydown", handleKeyDown);
			document.removeEventListener("keyup", handleKeyUp);
		};
	});

	function wheel(ctx, x, y, angle) {
		ctx.save(); // save current state

		// move origin to wheel center
		ctx.translate(x, y);

		// rotate canvas
		ctx.rotate(angle);

		// draw wheel centered at (0,0)
		ctx.beginPath();
		ctx.arc(0, 0, 50, 0, 2 * Math.PI);
		ctx.fillStyle = 'black';
		ctx.fill();
		ctx.stroke();

		// (optional) spoke to show rotation
		
		ctx.beginPath();
		ctx.strokeStyle = 'rgb(200, 200, 200)';
		ctx.moveTo(0, 0);
		ctx.lineTo(40, 30);
		ctx.stroke();
		ctx.restore();
	}

	function bike(ctx, x, y, angle) {

		ctx.save();
		ctx.translate(x, y);

		ctx.strokeStyle = 'black';

		ctx.rotate(angle);

		ctx.beginPath();
		ctx.arc(315, 0, 50, 0, 2 * Math.PI);
		ctx.fillStyle = 'black';
		ctx.fill();
		ctx.stroke();

		ctx.fillStyle = 'rgb(200, 200, 200)';
		ctx.strokeStyle = 'rgb(200, 200, 200)';
		ctx.beginPath();
		ctx.moveTo(100, -20);
		ctx.lineTo(100, -30);
		ctx.lineTo(-5, -5);
		ctx.lineTo(-2.5, 5);
		ctx.lineTo(100, -20);
		ctx.fill();
		ctx.stroke();

		ctx.fillStyle = 'rgb(255, 81, 0)';
		ctx.strokeStyle = 'rgb(255, 81, 0)';

		ctx.beginPath();
		ctx.moveTo(0, -120);
		ctx.lineTo(100, -100);
		ctx.lineTo(200, -100);
		ctx.lineTo(250, -120);
		ctx.lineTo(240, -140);
		ctx.lineTo(250, -145);
		//rita en stänkskärm oxå
		ctx.lineTo(315, 0);
		ctx.lineTo(305, 5);
		ctx.lineTo(260, -100);
		ctx.lineTo(200, -20);
		ctx.lineTo(100, -20);
		ctx.lineTo(100, -50);
		ctx.lineTo(0, -120);
		ctx.stroke();
		ctx.fill();

		ctx.restore();

	};

	function ground(ctx) {
		ctx.fillStyle = 'rgb(100, 100, 100)';
		ctx.fillRect(0, 500, 1000, 200);
	}

	function update() {
		ctx.clearRect(0, 0, 1000, 700);

		ground(ctx);

		if (keys.ArrowLeft) {
			console.log("Left pressed");
		};
		if (keys.ArrowRight) {
			console.log("Right pressed");
		};
		if (keys.ArrowUp) {
			console.log("Up pressed");
			angularVelocity += acceleration;
		};
		if (keys.ArrowDown) {
			console.log("Down pressed");
			angularVelocity -= acceleration;
		};

		// apply friction (slows down over time)
		angularVelocity *= friction;

		// update rotation
		wheelAngle += angularVelocity;
		//console.log(angularVelocity);

		if (angularVelocity <= 0.002) {
			angularVelocity = 0;
		};

		wheel(ctx, x, y, wheelAngle);
		bike(ctx, x, y, angle);

		requestAnimationFrame(update);
	};

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
