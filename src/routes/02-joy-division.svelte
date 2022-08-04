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

		const step = 10;
		const lines = [];

		for (let i = step; i <= size - step; i += step) {
			const line = [];
			for (let j = step; j <= size - step; j += step) {
				const distanceToCenter = Math.abs(j - size / 2);
				const variance = Math.max(size / 2 - 50 - distanceToCenter, 0);
				const random = (-Math.random() * variance) / 2;
				const point = { x: j, y: i + random };
				line.push(point);
			}
			lines.push(line);
		}

		let i;
		for (i = 5; i < lines.length; ++i) {
			context.beginPath();
			context.moveTo(lines[i][0].x, lines[i][0].y);

			let j;
			for (j = 0; j < lines[i].length - 2; ++j) {
				const xc = (lines[i][j].x + lines[i][j + 1].x) / 2;
				const yc = (lines[i][j].y + lines[i][j + 1].y) / 2;
				context.quadraticCurveTo(lines[i][j].x, lines[i][j].y, xc, yc);
			}

			context.quadraticCurveTo(lines[i][j].x, lines[i][j].y, lines[i][j + 1].x, lines[i][j + 1].y);
			context.save();
			context.globalCompositeOperation = 'destination-out';
			context.fill();
			context.restore();
			context.stroke();
		}
	});
</script>

<Frame>
	<Canvas bind:canvas />
</Frame>
