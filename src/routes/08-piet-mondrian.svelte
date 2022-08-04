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
		context.lineWidth = 8;

		const step = size / 7;
		const white = '#F2F5F1';
		const colors = ['#D40920', '#1356A2', '#F7D842'];

		/** @type {{ x: number; y: number; width: number; height: number; color?: string }[]} */
		const squares = [
			{
				x: 0,
				y: 0,
				width: size,
				height: size
			}
		];

		for (let i = 0; i < size; i += step) {
			splitSquaresWith({ y: i });
			splitSquaresWith({ x: i });
		}

		draw();

		function draw() {
			for (let i = 0; i < colors.length; ++i) {
				squares[Math.floor(Math.random() * squares.length)].color = colors[i];
			}
			for (let i = 0; i < squares.length; ++i) {
				context.beginPath();
				context.rect(squares[i].x, squares[i].y, squares[i].width, squares[i].height);
				if (squares[i].color) {
					context.fillStyle = /** @type string */ (squares[i].color);
				} else {
					context.fillStyle = white;
				}
				context.fill();
				context.stroke();
			}
		}

		/** @param {{ y?: number; x?: number; }} coordinates */
		function splitSquaresWith(coordinates) {
			const { x, y } = coordinates;

			for (let i = squares.length - 1; i >= 0; --i) {
				const square = squares[i];

				if (x && x > square.x && x < square.x + square.width) {
					if (Math.random() > 0.5) {
						squares.splice(i, 1);
						splitOnX(square, x);
					}
				}

				if (y && y > square.y && y < square.y + square.height) {
					if (Math.random() > 0.5) {
						squares.splice(i, 1);
						splitOnY(square, y);
					}
				}
			}
		}

		/**
		 * @param {{ x: number; y: number; width: number; height: number; color?: string; }} square
		 * @param {number} splitAt
		 */
		function splitOnX(square, splitAt) {
			const squareA = {
				x: square.x,
				y: square.y,
				width: square.width - (square.width - splitAt + square.x),
				height: square.height
			};

			const squareB = {
				x: splitAt,
				y: square.y,
				width: square.width - splitAt + square.x,
				height: square.height
			};

			squares.push(squareA);
			squares.push(squareB);
		}

		/**
		 * @param {{ x: number; y: number; width: number; height: number; color?: string; }} square
		 * @param {number} splitAt
		 */
		function splitOnY(square, splitAt) {
			const squareA = {
				x: square.x,
				y: square.y,
				width: square.width,
				height: square.height - (square.height - splitAt + square.y)
			};

			const squareB = {
				x: square.x,
				y: splitAt,
				width: square.width,
				height: square.height - splitAt + square.y
			};

			squares.push(squareA);
			squares.push(squareB);
		}
	});
</script>

<Frame>
	<Canvas bind:canvas />
</Frame>
