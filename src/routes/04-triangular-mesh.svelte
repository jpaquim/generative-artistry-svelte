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
		context.lineJoin = 'bevel';

		const lines = [];
		const gap = size / 8;
		let odd;

		for (let y = gap / 2; y <= size; y += gap) {
			odd = !odd;
			const line = [];
			for (let x = gap / 4; x <= size; x += gap) {
				// const dot = { x: x + (odd ? gap / 2 : 0), y };
				// context.beginPath();
				// context.arc(dot.x, dot.y, 1, 0, 2 * Math.PI, true);
				line.push({
					x: x + (Math.random() * 0.8 - 0.4) * gap + (odd ? gap / 2 : 0),
					y: y + (Math.random() * 0.8 - 0.4) * gap
				});
				context.fill();
			}
			lines.push(line);
		}

		odd = true;

		for (let y = 0; y < lines.length - 1; ++y) {
			odd = !odd;
			const dotLine = [];
			for (let i = 0; i < lines[y].length; ++i) {
				dotLine.push(odd ? lines[y][i] : lines[y + 1][i]);
				dotLine.push(odd ? lines[y + 1][i] : lines[y][i]);
			}
			for (let i = 0; i < dotLine.length - 2; ++i) {
				drawTriangle(dotLine[i], dotLine[i + 1], dotLine[i + 2]);
			}
		}
	});

	/**
	 * @param {{ x: number; y: number; }} pointA
	 * @param {{ x: number; y: number; }} pointB
	 * @param {{ x: number; y: number; }} pointC
	 */
	function drawTriangle(pointA, pointB, pointC) {
		context.beginPath();
		context.moveTo(pointA.x, pointA.y);
		context.lineTo(pointB.x, pointB.y);
		context.lineTo(pointC.x, pointC.y);
		context.lineTo(pointA.x, pointA.y);
		context.closePath();
		const gray = Math.floor(Math.random() * 16).toString(16);
		context.fillStyle = `#${gray}${gray}${gray}`;
		context.fill();
		context.stroke();
	}
</script>

<Frame>
	<Canvas bind:canvas />
</Frame>
