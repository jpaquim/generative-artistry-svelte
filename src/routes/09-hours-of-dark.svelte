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

		const cols = 23;
		const rows = 16;
		const days = 365;

		const gridw = size * 0.9;
		const gridh = size * 0.7;
		const cellw = gridw / cols;
		const cellh = gridh / rows;
		const margx = (size - gridw) * 0.5;
		const margy = (size - gridh) * 0.5;

		for (let i = 0; i < days; ++i) {
			const col = Math.floor(i / rows);
			const row = i % rows;

			const x = margx + col * cellw;
			const y = margy + row * cellh;
			const w = 2;
			const h = 30;

			context.save();
			context.translate(x, y);

			context.beginPath();
			context.rect(0, 0, cellw, cellh);
			context.clip();

			context.translate(cellw * 0.5, cellh * 0.5);

			const phi = (i / days) * Math.PI;
			const theta = Math.sin(phi) * Math.PI * 0.45 + 0.85;

			context.rotate(theta);

			const scale = Math.abs(Math.cos(phi)) * 2 + 1;

			context.scale(scale, 1);

			context.beginPath();
			context.rect(w * -0.5, h * -0.5, w, h);
			context.fill();

			context.restore();
		}
	});
</script>

<Frame>
	<Canvas bind:canvas />
</Frame>
