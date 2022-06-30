<script>
	import { browser } from '$app/env';
	import Members from '../lib/Members.svelte';
	import SimulationPlayer from '../lib/SimulationPlayer.svelte';
	import { onMount } from 'svelte';

	let index = 0;
	/**
	 * @type {HTMLCollectionOf<HTMLElement>}
	 */
	let sections = [];

	onMount(() => {
		sections = document.getElementsByTagName('section');
	});

	$: if (browser) {
		const section = sections[index];
		if (section) {
			section.scrollIntoView({ behavior: 'smooth' });
		}
	}
</script>

<svelte:window
	on:keydown={(e) => {
		switch (e.key) {
			case ' ':
			case 'ArrowDown':
				e.preventDefault();
				index = (index + 1) % sections.length;
				break;

			case 'ArrowUp':
				e.preventDefault();
				index = (index - 1 + sections.length) % sections.length;
				break;

			default:
				break;
		}
	}}
/>

<section>
	<h1 style="text-align: center; margin-bottom: 0;">Voltron</h1>
	<h2 style="text-align: center; font-weight: 400; font-size: 1em;">Virtuality</h2>

	<Members />
</section>

<section>
	<img
		class="tfl"
		src="https://upload.wikimedia.org/wikipedia/commons/thumb/7/7d/TfL_mark.svg/1200px-TfL_mark.svg.png"
		alt=""
	/>
</section>

<section>
	<h2 style="text-align: center;">Simulation de condition d’accident</h2>

	<SimulationPlayer />
</section>

<section>
	<h2 style="text-align: center; margin-bottom: 1em;">Des technologies adaptées</h2>

	<div class="techs">
		<img src="https://upload.wikimedia.org/wikipedia/commons/c/c4/Unity_2021.svg" alt="" />
		<img
			src="https://upload.wikimedia.org/wikipedia/commons/thumb/3/3c/Logo_Blender.svg/1280px-Logo_Blender.svg.png"
			alt=""
		/>
	</div>
</section>

<!-- Cloud: Sécurisé (Confidentialité), Indépendant des autres infrastructure TFL (Ségrégation de données), service autonome (Automatisé).

Sécurity: Données sécurisées et chiffrées, instruction des solutions de sécurité clair (Doc), Définir plusieurs niveau d’accès et géré tout type d’erreur involontaire (Système de droit).

IOT: Capteur pour récolter les données les plus utiles possibles, Les capteurs sont robuste, flexible, non dangereux et surveillé (Manuel ou interface ??),  il faut « Proof of Concept » doit déterminer les endroits précis des capteurs, leurs tailles, leurs fonction et leurs protection

BigData: Donnée store dans une BDD maison, Interface pour analysée, synthétisé et affichée en direct (Dashboard), Toute données non habituel ferait object d’alarme.

AI: Algorithme de simulation réaliste de plusieurs trajectoire possible de véhicule, s’améliore avec l’accumulation de données (Machine Learning) -->
<style lang="scss">
	section {
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
		height: 100vh;
		padding: 2em 10vw;
	}

	img.tfl {
		width: 100%;
	}

	.techs {
		display: flex;
		gap: 2em;

		img {
			width: 100%;
			min-width: 0;
			object-fit: contain;
		}
	}
</style>
