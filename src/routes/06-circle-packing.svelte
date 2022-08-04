<script>
	import { onMount } from 'svelte';
	import Canvas from '$lib/Canvas.svelte';
	import Frame from '$lib/Frame.svelte';

	/** @type HTMLCanvasElement */
	let canvas;
	/** @type CanvasRenderingContext2D */
	let context;

	onMount(() => {
		context = /** @type CanvasRenderingContext2D */ (canvas.getContext('2d'));

		const size = canvas.clientWidth;
		const dpr = window.devicePixelRatio;
		canvas.width = size * dpr;
		canvas.height = size * dpr;
		context.scale(dpr, dpr);

		context.lineWidth = 2;

		/** @type {{ x: number; y: number; radius: number; }[]} */
		const circles = [];
		const minRadius = 2;
		const maxRadius = 100;
		const totalCircles = 500;
		const createCircleAttempts = 500;

		function createAndDrawCircle() {
			/** @type {{ x: number; y: number; radius: number; }}*/
			let newCircle;
			let circleSafeToDraw = false;
			let tries = 0;
			do {
				newCircle = {
					x: Math.floor(Math.random() * size),
					y: Math.floor(Math.random() * size),
					radius: minRadius
				};

				if (!doesCircleHaveACollision(newCircle)) {
					circleSafeToDraw = true;
					break;
				}

				++tries;
			} while (tries < createCircleAttempts);

			if (!circleSafeToDraw) {
				return;
			}

			for (let radiusSize = minRadius; radiusSize < maxRadius; ++radiusSize) {
				newCircle.radius = radiusSize;
				if (doesCircleHaveACollision(newCircle)) {
					--newCircle.radius;
					break;
				}
			}

			circles.push(newCircle);
			context.beginPath();
			context.arc(newCircle.x, newCircle.y, newCircle.radius, 0, 2 * Math.PI);
			context.stroke();
		}

		/** @param {{ x: number; y: number; radius: number; }} circle */
		function doesCircleHaveACollision(circle) {
			for (let i = 0; i < circles.length; ++i) {
				const otherCircle = circles[i];
				const a = circle.radius + otherCircle.radius;
				const x = circle.x - otherCircle.x;
				const y = circle.y - otherCircle.y;

				if (a >= Math.sqrt(x * x + y * y)) {
					return true;
				}
			}

			if (circle.x + circle.radius >= size || circle.x - circle.radius <= 0) {
				return true;
			}

			if (circle.y + circle.radius >= size || circle.y - circle.radius <= 0) {
				return true;
			}

			return false;
		}

		for (let i = 0; i < totalCircles; ++i) {
			createAndDrawCircle();
		}
	});
</script>

<Frame>
	<Canvas bind:canvas />
</Frame>
