<script lang="ts">
	import { i18n } from '$lib/i18n';
	import { ParaglideJS } from '@inlang/paraglide-sveltekit';

	import Grain from '$lib/Grain.svelte';

	let { children } = $props();
</script>

<ParaglideJS {i18n}>
	<Grain intensity={50} grainSize={1} frameRate={10} />
	{@render children()}
</ParaglideJS>

<style>
	:global {
		@import url('https://fonts.googleapis.com/css2?family=Dela+Gothic+One&family=Roboto:ital,wght@0,100;0,300;0,400;0,500;0,700;0,900;1,100;1,300;1,400;1,500;1,700;1,900&display=swap');
		body {
			min-height: 100vh;

			background-color: hsla(60, 55%, 38%, 1);
			background-image: radial-gradient(at 79% 57%, hsla(103, 85%, 74%, 1) 0px, transparent 50%),
				radial-gradient(at 99% 69%, hsla(73, 77%, 70%, 1) 0px, transparent 50%),
				radial-gradient(at 7% 36%, hsla(39, 93%, 61%, 1) 0px, transparent 50%),
				radial-gradient(at 56% 24%, hsla(18, 91%, 68%, 1) 0px, transparent 50%),
				radial-gradient(at 91% 30%, hsla(158, 82%, 70%, 1) 0px, transparent 50%),
				radial-gradient(at 44% 13%, hsla(297, 99%, 62%, 1) 0px, transparent 50%),
				radial-gradient(at 65% 11%, hsla(42, 81%, 62%, 1) 0px, transparent 50%),
				radial-gradient(at 19% 73%, hsla(102, 64%, 70%, 1) 0px, transparent 50%);
			font-family: 'Dela Gothic One', sans-serif;

			color: black;
			overflow: hidden;
		}
	}
</style>
