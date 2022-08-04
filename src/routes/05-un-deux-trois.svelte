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

		context.lineWidth = 4;
		context.lineCap = 'round';

		const step = 20;
		const aThirdOfHeight = size / 3;

		for (let y = step; y < size - step; y += step) {
			for (let x = step; x < size - step; x += step) {
				if (y < aThirdOfHeight) {
					draw(x, y, step, step, [0.5]);
				} else if (y < aThirdOfHeight * 2) {
					draw(x, y, step, step, [0.2, 0.8]);
				} else {
					draw(x, y, step, step, [0.1, 0.5, 0.9]);
				}
			}
		}
	});

	/**
	 * @param {number} x
	 * @param {number} y
	 * @param {number} width
	 * @param {number} height
	 * @param {number[]} positions
	 */
	function draw(x, y, width, height, positions) {
		context.save();
		context.translate(x + width / 2, y + height / 2);
		context.rotate(Math.random() * 5);
		context.translate(-width / 2, -height / 2);

		for (let i = 0; i <= positions.length; ++i) {
			context.beginPath();
			context.moveTo(positions[i] * width, 0);
			context.lineTo(positions[i] * width, height);
			context.stroke();
		}

		context.restore();
	}
</script>

<Frame>
	<Canvas bind:canvas />
</Frame>
