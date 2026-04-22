<script>
	import { onMount } from 'svelte';

	const keys = {
		ArrowLeft: false,
		ArrowRight: false,
		ArrowUp: false,
		ArrowDown: false
	};

	let ctx;
	let gradient;
	let lampGradient;
	let bikeAngle = 0;
	let bikeAngularVelocity = 0;
	let wheelAngle = 0;
	let angularVelocity = 0;
	let velocity = 0;
	let treeVelocity = 0;
	let x = 200;
	let y = 575;
	let points = 0;
	let highPoint = 0;
	let roadmarkings = [];
	let bikeColor = 'rgb(255, 81, 0)';

	const acceleration = 0.007;
	const wheelieAcceleration = 0.01;
	const friction = 0.98;
	const gravity = 0.0035;

	onMount(() => {
		const canvas = document.querySelector('#myCanvas');
		ctx = canvas.getContext('2d');

		gradient = ctx.createLinearGradient(0, 0, 0, 700);

		gradient.addColorStop(0, "#6a0d83");
		gradient.addColorStop(0.20, "#ce4993");
		gradient.addColorStop(0.40, "#ee5d6c");
		gradient.addColorStop(0.60, "#fb9062");
		gradient.addColorStop(0.80, "#eeaf61");
		gradient.addColorStop(1, "#eeaf61");

		lampGradient = ctx.createLinearGradient(0, 100, 0, 500);

		lampGradient.addColorStop(0, "lightyellow");
		lampGradient.addColorStop(1, "rgba(255, 255, 240, 0)");

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

		document.addEventListener('keydown', handleKeyDown);
		document.addEventListener('keyup', handleKeyUp);

		roadmarkings.push(new Roadmarks(116.5, 600));
		roadmarkings.push(new Roadmarks(449.5, 600));
		roadmarkings.push(new Roadmarks(782.5, 600));

		update();

		return () => {
			document.removeEventListener('keydown', handleKeyDown);
			document.removeEventListener('keyup', handleKeyUp);
		};
	});

	class Roadmarks {
		constructor(startingX, startingY) {
			this.startingX = startingX;
			this.startingY = startingY;
			this.length = 100;
			this.height = 20;
			this.leftSide = this.startingX + this.length;
			this.velocity = 0;
		}

		update(ctx, angVelocity) {
			this.velocity -= angVelocity;
			this.roadVelocity = this.velocity * 60;
			this.position = this.roadVelocity + this.startingX;

			ctx.save();

			// draw roadmark
			ctx.fillStyle = 'white';
			ctx.strokeStyle = 'white';
			ctx.beginPath();
			this.checkEdge();
			ctx.rect(this.position, this.startingY, this.length, this.height);
			ctx.fill();
			ctx.stroke();
			ctx.restore();
		}

		checkEdge() {
			if (this.position + this.length < 0) {
				this.startingX += 1100;
			}
		}
	}

	function tree(ctx, velocity) {
		let treeVelocity = velocity * 50;
		ctx.save();

		ctx.fillStyle = 'gray';
		ctx.strokeStyle = 'lightgray';
		ctx.beginPath();
		ctx.moveTo(treeVelocity + 800, 400);
		ctx.lineTo(treeVelocity + 810, 350);
		ctx.lineTo(treeVelocity + 810, 100);
		ctx.lineTo(treeVelocity + 830, 100);
		ctx.lineTo(treeVelocity + 830, 350);
		ctx.lineTo(treeVelocity + 840, 400);
		ctx.lineTo(treeVelocity + 840, 550);
		ctx.lineTo(treeVelocity + 800, 550);
		ctx.fill();
		ctx.stroke();

		ctx.fillStyle = 'lightyellow';
		ctx.strokeStyle = 'lightyellow';
		ctx.beginPath();
		ctx.ellipse(treeVelocity + 820, 110, 15, 5, 0, 0, 2 * Math.PI);
		ctx.fill();
		ctx.stroke();

		ctx.beginPath();
		ctx.fillStyle = lampGradient;
		ctx.strokeStyle = lampGradient;

		ctx.moveTo(treeVelocity + 810, 100);
		ctx.lineTo(treeVelocity + 830, 100);
		ctx.lineTo(treeVelocity + 900, 500);
		ctx.lineTo(treeVelocity + 740, 500);
		ctx.lineTo(treeVelocity + 810, 100);
		ctx.fill();
		ctx.stroke();

		ctx.beginPath();
		ctx.fillStyle = 'gray';
		ctx.strokeStyle = 'lightgray';

		ctx.moveTo(treeVelocity + 810, 100);
		ctx.lineTo(treeVelocity + 830, 100);
		ctx.lineTo(treeVelocity + 840, 110);
		ctx.lineTo(treeVelocity + 800, 110);
		ctx.lineTo(treeVelocity + 810, 100);

		ctx.fill();
		ctx.stroke();

		ctx.restore();
	};

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
		ctx.lineTo(25, 25);
		ctx.moveTo(0, 0);
		ctx.lineTo(-25, 25);
		ctx.moveTo(0, 0);
		ctx.lineTo(-25, -25);
		ctx.moveTo(0, 0);
		ctx.lineTo(25, -25);
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

		ctx.fillStyle = bikeColor;
		ctx.strokeStyle = bikeColor;

		ctx.beginPath();
		ctx.moveTo(0, -120);
		ctx.lineTo(100, -100);
		ctx.lineTo(200, -100);
		ctx.lineTo(250, -120);
		ctx.lineTo(240, -140);
		ctx.lineTo(250, -145);
		//rita en stänkskärm oxå
		ctx.lineTo(275, -90);
		ctx.lineTo(330, -85);
		ctx.lineTo(350, -70);
		ctx.lineTo(325, -70);
		ctx.lineTo(320, -75);
		ctx.lineTo(282.5, -75);
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
	}

	function ground(ctx) {
		ctx.fillStyle = 'rgb(100, 100, 100)';
		ctx.fillRect(0, 500, 1000, 200);
	}

	function score(points, x, y) {
		ctx.fillStyle = 'rgb(200, 146, 0)';
		ctx.font = '40px Arial';
		ctx.fillText(points.toString(), 482, 202);
		ctx.fillText(points.toString(), 483, 203);
		ctx.fillStyle = 'rgb(245, 191, 3)';
		ctx.fillText(points.toString(), 480, 200);
	}

	function highScore(points, x, y) {
		ctx.fillStyle = 'rgb(200, 146, 0)';
		ctx.font = '40px Arial';
		ctx.fillText('High Score: ' + points.toString(), 18, 48);
		ctx.fillText('High Score: ' + points.toString(), 17, 47);
		ctx.fillStyle = 'rgb(245, 191, 3)';
		ctx.fillText('High Score: ' + points.toString(), 20, 50);
	}

	function update() {
		ctx.clearRect(0, 0, 1000, 700);

		ctx.fillStyle = gradient;
		ctx.fillRect(0, 0, 1000, 700)

		ground(ctx);

		if (angularVelocity >= 0.008) {
			if (keys.ArrowLeft) {
				console.log('Left pressed');
				//bikeAngularVelocity -= wheelieAcceleration;
				if (bikeAngle <= 0 && bikeAngle > -0.9) {
					bikeAngularVelocity -= wheelieAcceleration * (1 * (bikeAngle + 0.9));
				} else {
					bikeAngularVelocity -= wheelieAcceleration * -(1 * (bikeAngle + 0.9));
				}
			}
			if (keys.ArrowRight) {
				console.log('Right pressed');
				//bikeAngularVelocity += wheelieAcceleration;
				if (bikeAngle <= 0 && bikeAngle > -0.9) {
					bikeAngularVelocity += wheelieAcceleration * (1 * (bikeAngle + 0.9));
				} else {
					bikeAngularVelocity += wheelieAcceleration * -(1 * (bikeAngle + 0.9));
				}
			}
		}
		if (keys.ArrowUp) {
			console.log('Up pressed');
			angularVelocity += acceleration;
		}
		if (keys.ArrowDown) {
			console.log('Down pressed');
			angularVelocity -= acceleration;
		}

		angularVelocity *= friction;

		if (bikeAngle < 0 && bikeAngle > -0.9) {
			bikeAngularVelocity += gravity * (bikeAngle + 0.9);
		} else {
			bikeAngularVelocity -= gravity * -(bikeAngle + 0.9) * 0.9;
		}

		if (bikeAngularVelocity <= -0.05) {
			bikeAngularVelocity = -0.05;
		}

		bikeAngle += bikeAngularVelocity;
		//console.log(bikeAngularVelocity);
		bikeAngularVelocity *= 0.99;

		velocity -= angularVelocity;
		treeVelocity -= angularVelocity;
		wheelAngle += angularVelocity;
		//console.log(velocity);

		points += Math.round(-bikeAngle + angularVelocity);

		if (angularVelocity <= 0.002) {
			angularVelocity = 0;
		}

		if (bikeAngle > 0) {
			bikeAngle = 0;
			bikeAngularVelocity = 0;

			if (points > highPoint) {
				highPoint = points;
			}
			points = 0;
		}
		if (bikeAngle < -3) {
			bikeAngle = -3;
			bikeAngularVelocity = 0;
			angularVelocity = 0;

			if (points > highPoint) {
				highPoint = points;
			}
			points = 0;
		}

		if (treeVelocity <= -17) {
			treeVelocity += 22;
		}

		if (highPoint > 5000) {
			bikeColor = 'rgb(82, 14, 125)'
		}

		tree(ctx, treeVelocity);
		for (const mark of roadmarkings) {
			mark.update(ctx, angularVelocity);
		}
		wheel(ctx, x, y, wheelAngle);
		bike(ctx, x, y, bikeAngle);
		score(points, x, y);
		highScore(highPoint, x, y);

		requestAnimationFrame(update);
	}
</script>

<canvas
	id="myCanvas"
	class="m-auto"
	width="1000"
	height="700"
	style="image-rendering: pixelated;"
></canvas>

<section id="hometext" class="text-lg font-bold mt-32">
	<section class="text-2xl">Controls:</section>
	<p>⬆️ to accelerate</p>
	<p>⬇️ to brake</p>
	<p>⬅️ and ➡️ to control bike</p>
</section>

<style>
	canvas {
		border: 2px solid rgb(255, 255, 255);
	}
</style>
