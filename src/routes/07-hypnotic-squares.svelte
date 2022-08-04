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

		const finalSize = 3;
		/** @type number */
		let startSteps;
		const offset = 2;
		const tileStep = (size - offset * 2) / 7;
		const startSize = tileStep;
		const directions = [-1, 0, 1];

		for (let x = offset; x < size - offset; x += tileStep) {
			for (let y = offset; y < size - offset; y += tileStep) {
				startSteps = 2 + Math.ceil(Math.random() * 3);
				const xDirection = directions[Math.floor(Math.random() * directions.length)];
				const yDirection = directions[Math.floor(Math.random() * directions.length)];
				draw(x, y, startSize, startSize, xDirection, yDirection, startSteps - 1);
			}
		}

		/**
		 * @param {number} x
		 * @param {number} y
		 * @param {number} width
		 * @param {number} height
		 * @param {number} xMovement
		 * @param {number} yMovement
		 * @param {number} steps
		 */
		function draw(x, y, width, height, xMovement, yMovement, steps) {
			context.beginPath();
			context.rect(x, y, width, height);
			context.stroke();

			if (steps >= 0) {
				const newSize = startSize * (steps / startSteps) + finalSize;
				let newX = x + (width - newSize) / 2;
				let newY = y + (height - newSize) / 2;
				newX = newX - ((x - newX) / (steps + 2)) * xMovement;
				newY = newY - ((y - newY) / (steps + 2)) * yMovement;
				draw(newX, newY, newSize, newSize, xMovement, yMovement, steps - 1);
			}
		}
	});
</script>

<Frame>
	<Canvas bind:canvas />
</Frame>
