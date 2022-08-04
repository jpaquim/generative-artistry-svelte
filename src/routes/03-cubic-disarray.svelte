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

		const randomDisplacement = 15;
		const rotateMultiplier = 20;
		const offset = 10;
		const squareSize = 30;

		for (let i = squareSize; i <= size - squareSize; i += squareSize) {
			for (let j = squareSize; j <= size - squareSize; j += squareSize) {
				let plusOrMinus = Math.random() < 0.5 ? -1 : 1;
				const rotateAmt =
					(((j / size) * Math.PI) / 180) * plusOrMinus * Math.random() * rotateMultiplier;
				plusOrMinus = Math.random() < 0.5 ? -1 : 1;
				const translateAmt = (j / size) * plusOrMinus * Math.random() * randomDisplacement;

				context.save();
				context.translate(i + translateAmt + offset, j + offset);
				context.rotate(rotateAmt);
				draw(squareSize, squareSize);
				context.restore();
			}
		}
	});

	/**
	 * @param {number} width
	 * @param {number} height
	 */
	function draw(width, height) {
		context.beginPath();
		context.rect(-width / 2, -height / 2, width, height);
		context.stroke();
	}
</script>

<Frame>
	<Canvas bind:canvas />
</Frame>
