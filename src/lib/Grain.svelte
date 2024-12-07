<script>
	import { onMount } from 'svelte';

	export let intensity = 30; // Default alpha value for grain (opacity)
	export let grainSize = 2; // Controls the size of the grains (1 = very small, higher = larger)
	export let frameRate = 5; // Reduced frames per second (lower number = slower noise)
	export let frameInterval = 100; // Interval to switch frames (in milliseconds)

	let canvas;
	let ctx;
	let frameCache = []; // Array to store generated frames
	let currentFrame = 0; // Index to track the current frame
	let loading = true; // Track loading state
	let loadingElement; // Loading element reference

	onMount(() => {
		// Initialize the canvas context
		ctx = canvas.getContext('2d');
		const width = window.innerWidth;
		const height = window.innerHeight;

		// Set the canvas size to match the viewport
		canvas.width = width;
		canvas.height = height;

		// Function to generate the grain effect
		function generateGrain() {
			const imageData = ctx.createImageData(width, height); // Create an empty imageData object
			const pixels = imageData.data;

			// Loop through all pixels and add noise
			for (let y = 0; y < height; y += grainSize) {
				for (let x = 0; x < width; x += grainSize) {
					const value = Math.random() * 255; // Generate a random grayscale value (0-255)

					// Loop to apply noise to a "grainSize" block of pixels
					for (let dy = 0; dy < grainSize; dy++) {
						for (let dx = 0; dx < grainSize; dx++) {
							const index = ((y + dy) * width + (x + dx)) * 4; // Calculate pixel index in the imageData array
							if (index < pixels.length) {
								pixels[index] = value; // Red
								pixels[index + 1] = value; // Green
								pixels[index + 2] = value; // Blue
								pixels[index + 3] = intensity; // Alpha (opacity)
							}
						}
					}
				}
			}

			return imageData; // Return the generated imageData
		}

		// Generate a frame and store it in the cache
		function createFrame() {
			const grain = generateGrain(); // Generate grain for the frame
			frameCache.push(grain); // Store the frame in the cache
			if (frameCache.length > frameRate) {
				frameCache.shift(); // Keep only the latest 'frameRate' number of frames
			}
		}

		// Create initial frames asynchronously (reduce loading time)
		async function preloadFrames() {
			for (let i = 0; i < frameRate; i++) {
				createFrame(); // Create initial frames to preload the cache
				await new Promise((resolve) => setTimeout(resolve, 50)); // Small delay between frame generation
			}
			loading = false; // Mark loading as complete
			renderGrain(); // Start rendering after loading is complete
		}

		// Function to render the cached frames with a delay
		function renderGrain() {
			if (!loading) {
				// Use the current frame from the cache
				if (frameCache.length > 0) {
					ctx.putImageData(frameCache[currentFrame], 0, 0);
					currentFrame = (currentFrame + 1) % frameCache.length; // Cycle through the frames
				}
			}
			setTimeout(renderGrain, frameInterval); // Wait for frameInterval before rendering next frame
		}

		// Start preloading frames
		preloadFrames();
	});
</script>

<!-- Loading screen with logo (text) and pulsing text -->
{#if loading}
	<div class="loading-screen">
		<div class="logo-container">
			<h1 class="logo">Alpaar</h1>
			<p class="loading-text">Loading...</p>
		</div>
	</div>
{/if}

<canvas bind:this={canvas}></canvas>

<style>
	canvas {
		position: fixed;
		top: 0;
		left: 0;
		width: 100vw;
		height: 100vh;
		pointer-events: none;
		z-index: -1;
		mix-blend-mode: overlay; /* Ensures it blends dynamically with the background */
	}

	/* Loading Screen styles */
	.loading-screen {
		position: fixed;
		top: 0;
		left: 0;
		width: 100vw;
		height: 100vh;
		display: flex;
		align-items: center;
		justify-content: center;
		background: black; /* Set background to white */
		z-index: 10;
		color: white; /* Black text on white background */
	}

	/* Logo container styles */
	.logo-container {
		text-align: center;
		opacity: 0;
		animation: fadeIn 1s forwards;
	}

	/* Logo style */
	.logo {
		font-size: 3rem;
		font-weight: bold;
		letter-spacing: 2px;
		margin-bottom: 20px;
	}

	/* Pulsing loading text style */
	.loading-text {
		font-size: 1.2rem;
		animation: pulse 1.5s infinite alternate;
	}

	@keyframes fadeIn {
		0% {
			opacity: 0;
		}
		100% {
			opacity: 1;
		}
	}

	@keyframes pulse {
		0% {
			opacity: 0.5;
		}
		100% {
			opacity: 1;
		}
	}
</style>
