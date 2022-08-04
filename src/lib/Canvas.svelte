<script>
	export let canvas;
</script>

<canvas bind:this={canvas} />

<style>
	canvas {
		box-shadow: inset 0 0 1px 0 rgba(0, 0, 0, 0.2);
		height: 320px;
		width: 320px;
		background-size: cover;
		background-position: center center;
	}
</style>
