<script>
	import { onMount } from 'svelte';
	
	let canvas, ctx;
	let drawing = false;
	let brushColor = '#000000';
	let brushWidth = 5;

	function startDrawing(event) {
			drawing = true;
			draw(event);
	}

	function endDrawing() {
			drawing = false;
			ctx.beginPath();
	}

	function draw(event) {
			if (!drawing) return;

			ctx.lineWidth = brushWidth;
			ctx.lineCap = 'round';
			ctx.strokeStyle = brushColor;

			ctx.lineTo(event.clientX - canvas.offsetLeft, event.clientY - canvas.offsetTop);
			ctx.stroke();
			ctx.beginPath();
			ctx.moveTo(event.clientX - canvas.offsetLeft, event.clientY - canvas.offsetTop);
	}

	function clearCanvas() {
			ctx.clearRect(0, 0, canvas.width, canvas.height);
	}

	function erase() {
			brushColor = '#FFFFFF'; // assuming the canvas background is white
	}

	function setColor(newColor) {
			brushColor = newColor;
	}

	function setWidth(newWidth) {
			brushWidth = newWidth;
	}

	function handleBlur() {
			endDrawing(); // stop drawing when canvas loses focus
	}

	onMount(() => {
			canvas = document.querySelector('canvas');
			ctx = canvas.getContext('2d');
	});
</script>

<style>
	canvas {
    border: 1px solid #ccc;
    display: block; /* ensures no margin issues */
    margin: auto; /* centers the canvas */
    background-color: #fff; /* white background */
	}

	.controls {
			text-align: center; /* centers the controls under the canvas */
			padding: 10px;
	}

	button, input[type='color'], input[type='range'] {
			padding: 5px;
			margin: 5px;
	}
</style>

<canvas
    bind:this={canvas}
    width="800"
    height="600"
    tabindex="0"
    on:mousedown={startDrawing}
    on:mouseup={endDrawing}
    on:mouseout={endDrawing}
    on:mousemove={draw}
    on:blur={endDrawing}>
</canvas>
<div class="controls">
    <input type="color" on:change={(e) => setColor(e.target.value)} value={brushColor} />
    <input type="range" min="1" max="20" on:change={(e) => setWidth(e.target.value)} value={brushWidth} />
    <button on:click={erase}>Eraser</button>
    <button on:click={clearCanvas}>Clear</button>
</div>
