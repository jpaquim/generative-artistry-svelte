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
		const step = 20;
		const dpr = window.devicePixelRatio;
		canvas.width = size * dpr;
		canvas.height = size * dpr;
		context.scale(dpr, dpr);

		context.lineCap = 'square';
		context.lineWidth = 2;

		for (let x = 0; x < size; x += step) {
			for (let y = 0; y < size; y += step) {
				draw(x, y, step, step);
			}
		}
	});

	/**
	 * @param {number} x
	 * @param {number} y
	 * @param {number} width
	 * @param {number} height
	 */
	function draw(x, y, width, height) {
		const leftToRight = Math.random() >= 0.5;

		if (leftToRight) {
			context.moveTo(x, y);
			context.lineTo(x + width, y + height);
		} else {
			context.moveTo(x + width, y);
			context.lineTo(x, y + height);
		}

		context.stroke();
	}
</script>

<Frame>
	<Canvas bind:canvas />
</Frame>
