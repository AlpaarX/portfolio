<script>
	// import { onMount } from 'svelte';
	// onMount(() => {
	// 	document.querySelectorAll('a').forEach((link) => {
	// 		link.addEventListener('hover', () => {
	// 			document.getElementById('audio').play();
	// 		});
	// 	});
	// });
</script>

<audio>
	<source src="src\lib\audio\hover.mp3" type="audio/mp3" id="audio" />
</audio>
<div class="grid">
	<div class="hero">
		<h1>デジタル未来へ <br /> Alpaar</h1>
	</div>
	<div class="links">
		<a href="https://github.com/AlpaarX">github</a>
		<a href="https://www.linkedin.com/in/dmitry-kasikhin/">linkedin</a>
	</div>
</div>

<style>
	.grid {
		display: grid;
		grid-template-columns: 1fr 1fr;
		grid-template-rows: 1fr;
		height: 100vh;
	}

	.grid > div {
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
	}

	h1 {
		font-size: 8rem;
		font-weight: normal;
		user-select: none;
	}
	.links {
		display: flex;
		gap: 5rem;
	}
	.links > a {
		font-size: 3rem;
		color: black;
		text-decoration: none;
	}
	.links > a:hover {
		text-decoration-line: underline;
	}
	@media (max-width: 1200px) {
		.grid {
			display: grid;
			grid-template-columns: 1fr;
			grid-template-rows: 1fr;
		}
		h1 {
			font-size: 3rem;
		}
	}
</style>
