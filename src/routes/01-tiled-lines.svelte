<script>
	import { onMount } from 'svelte';

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

<div class="border">
	<div class="frame">
		<canvas bind:this={canvas} />
	</div>
</div>

<style>
	.border {
		box-sizing: border-box;
		position: relative;
		background: black;
		background-image: linear-gradient(to left bottom, #111, #000);
		padding: 20px;
		box-shadow: -1px 1px var(--blur) 1px rgba(0, 0, 0, 0.1),
			-2px 2px var(--blur) 1px rgba(0, 0, 0, 0.09), -3px 3px var(--blur) 1px rgba(0, 0, 0, 0.08),
			-4px 4px var(--blur) 1px rgba(0, 0, 0, 0.07), -5px 5px var(--blur) 1px rgba(0, 0, 0, 0.06),
			-6px 6px var(--blur) 1px rgba(0, 0, 0, 0.05), -7px 7px var(--blur) 1px rgba(0, 0, 0, 0.04),
			-8px 8px var(--blur) 1px rgba(0, 0, 0, 0.03), -9px 9px var(--blur) 1px rgba(0, 0, 0, 0.03),
			-10px 10px var(--blur) 1px rgba(0, 0, 0, 0.03), -11px 11px var(--blur) 1px rgba(0, 0, 0, 0.03),
			-12px 12px var(--blur) 1px rgba(0, 0, 0, 0.02), -13px 13px var(--blur) 1px rgba(0, 0, 0, 0.02),
			-14px 14px var(--blur) 1px rgba(0, 0, 0, 0.01), -15px 15px var(--blur) 1px rgba(0, 0, 0, 0.01),
			-16px 16px var(--blur) 1px rgba(0, 0, 0, 0.01);
	}

	.frame {
		left: 3%;
		top: 2.5%;
		box-shadow: inset -1px 1px 6px 1px rgba(0, 0, 0, 0.24);
		background: white;
		align-items: center;
		display: flex;
		padding: 40px;
		box-sizing: border-box;
	}

	canvas {
		box-shadow: inset 0 0 1px 0 rgba(0, 0, 0, 0.2);
		height: 320px;
		width: 320px;
		background-size: cover;
		background-position: center center;
	}
</style>
