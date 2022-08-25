<style>
	.main {
		text-align: center;
	}

	:global(body) {
		overflow: hidden;
	}

	span {
		position: absolute;
		font-size: 5vw;
		user-select: none;
	}
</style>

<script context="module">
	import { siteTitle } from '../stores/_config.js';
	export async function preload() {

		const res = await this.fetch(`undefined.json`);
		const article = await res.json();

		return { article, siteTitle };
	}
</script>

<script>
	export let article;
	export let siteTitle;

	import { onMount } from 'svelte';

	let characters = ['💋','👄','💄','🥳', '🎉', '✨'];

	let confetti = new Array(100).fill()
		.map((_, i) => {
			return {
				character: characters[i % characters.length],
				x: Math.random() * 100,
				y: -20 - Math.random() * 100,
				r: 0.1 + Math.random() * 1
			};
		})
		.sort((a, b) => a.r - b.r);

	onMount(() => {
		let frame;

		function loop() {
			frame = requestAnimationFrame(loop);

			confetti = confetti.map(emoji => {
				emoji.y += 0.7 * emoji.r;
				if (emoji.y > 120) emoji.y = -20;
				return emoji;
			});
		}

		loop();

		return () => cancelAnimationFrame(frame);
	});


</script>

<svelte:head>
	<title>{siteTitle}</title>
</svelte:head>

{#each confetti as c}
	<span style="left: {c.x}%; top: {c.y}%; transform: scale({c.r})">{c.character}</span>
{/each}


<div class="main">
	{@html article.html}
</div>
