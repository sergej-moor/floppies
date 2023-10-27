<script lang="ts">
	import P5, { type Sketch } from 'p5-svelte';
	import { onMount } from 'svelte';

	let artboard;
	let sketch: Sketch;

	let artboardWidth = 300;
	let artboardHeight = 200;

	let canvas;
	let CCapture;
	let capturer: CCapture;
	onMount(async () => {
		const module = await import('$lib/CCapture.all.min.js');
		CCapture = module.default;
		// Use CCapture.js here
		capturer = new CCapture({ format: 'webm', framerate: 60, verbose: true });
	});

	sketch = (p5) => {
		p5.setup = () => {
			canvas = p5.createCanvas(artboardWidth, artboardHeight);
		};

		let colors = [
			'#d9ed92',
			'#b5e48c',
			'#99d98c',
			'#76c893',
			'#52b69a',
			'#34a0a4',
			'#168aad',
			'#1a759f',
			'#1e6091',
			'#184e77'
		];
		let yoffs = new Array(colors.length).fill(0.0);
		p5.draw = () => {
			if (p5.frameCount === 1) {
				capturer.start();
			}

			if (p5.frameCount < 60){
				capturer.capture(canvas)
			}else{
				capturer.save();
				capturer.stop();
			}

			p5.background(colors[9]);

			for (let index = 0; index < colors.length; index++) {
				const element = colors[index];
				drawWave(element, index, 100, 0.1 + index * 0.02, 0.01, 15 * index, 10);
			}

			/* drawWave('#252A34', 0, 40, 0.08, 0.01, -20);
			drawWave('#08D9D6', 1, 120, 0.1, 0.02);
			drawWave('#FF2E63', 1, 120, 0.1, 0.02, 50); */

			function drawWave(
				color: string,
				yoffsindex: number,
				height: number = 100,
				xdim: number = 0.05,
				ydim: number = 0.01,
				yoffset: number = 0,
				smoothness: number = 10
			) {
				p5.fill(color);
				p5.noStroke();
				// We are going to draw a polygon out of the wave points
				p5.beginShape();

				let xoff = 5; // Option #1: 2D Noise
				// let xoff = yoff; // Option #2: 1D Noise

				// Iterate over horizontal pixels
				for (let x = 0; x <= p5.width; x += smoothness) {
					// Calculate a y value according to noise, map to

					let y = p5.map(p5.noise(xoff, yoffs[yoffsindex]), 0, 1, height, 20);
					p5.vertex(x, y + yoffset - 20);
					// Increment x dimension for noise
					xoff += xdim;
				}
				// increment y dimension for noise
				yoffs[yoffsindex] += ydim;
				p5.vertex(p5.width, p5.height);
				p5.vertex(0, p5.height);
				p5.endShape(p5.CLOSE);
			}
		};
	};
</script>

<div class="justify-center flex p-8">
	<div bind:this={artboard} class={`max-w-[${artboardWidth}] bg-gray-500`}>
		<P5 {sketch} />
	</div>
</div>

<!-- <script lang="ts">
	let width: number;
	let height: number;
</script>

Paragraph width: {width}px Paragraph height: {height}px

<div class={`w-full h-[${width}px]`} bind:clientWidth={width} bind:clientHeight={height}>
	This is some example text.
</div>
 -->
