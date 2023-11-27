<script>
	import { Confetti } from 'svelte-confetti';

	import Matched from '../lib/Matched.svelte';
	import { fade } from 'svelte/transition';

	let isDrawing = false;
	let isStarted = false;
	let totalDraws = 0;

	let people = [
		'Abdurrahman',
		'Çağla',
		'Eray',
		'Esra',
		'Furkan',
		'Gökçe',
		'Kadir',
		'Kasım',
		'Melih',
		'Murat',
		'Orhan',
		'Önder',
		'Uğur',
		'Umutcan'
	];
	let originalPeople = [...people];

	let drawerPeople = [...people];
	let receiverPeople = [...people];
	let drawedPeople = [];
	let receivedPeople = [];
	let matchedPeople = [];

	function randomInteger(min, max) {
		return Math.floor(Math.random() * (max - min + 1)) + min;
	}

	function shuffleArray(array) {
		for (let i = array.length - 1; i > 0; i--) {
			const j = Math.floor(Math.random() * (i + 1));
			[array[i], array[j]] = [array[j], array[i]];
		}

		return array;
	}

	const draw = () => {
		isDrawing = true;
		let from = '';
		let to = '';

		drawerPeople = shuffleArray(drawerPeople);
		from = drawerPeople[0];
		drawerPeople = drawerPeople.filter((person) => person !== from);
		drawerPeople = [...drawerPeople];

		// Receiver
		receiverPeople = shuffleArray(receiverPeople);

		to = receiverPeople[0];

		if (from === to) {
			to = receiverPeople[1];
		}

		receiverPeople = receiverPeople.filter((person) => person !== to);
		receiverPeople = [...receiverPeople];

		setTimeout(() => {
			matchedPeople = [...matchedPeople, { from, to }];

			// Matched People
			drawedPeople = [...drawedPeople, from];
			receivedPeople = [...receivedPeople, to];

			isStarted = true;
			isDrawing = false;
			totalDraws += 1;
		}, 2000);
	};
</script>

<main>
	<div class="content">
		<h1 class="header">Pixelate 2024 Raffle</h1>

		<section>
			<h2 class="subheader">Katılımcılar</h2>
			<div class="people">
				{#each originalPeople as person}
					<span
						class="person"
						class:drawed={drawedPeople.includes(person)}
						class:received={receivedPeople.includes(person)}>{person}</span
					>
				{/each}
			</div>
		</section>

		<button
			class="draw-button"
			disabled={isDrawing || totalDraws === originalPeople.length}
			on:click={draw}
		>
			{#if isDrawing}
				Çekiliyor...
			{:else if totalDraws === originalPeople.length}
				Çekiliş Yapıldı
			{:else}
				Çekiliş Yap
			{/if}
		</button>

		<section class="matches">
			<h2 class="subheader">Eşleşmeler</h2>
			<div class="matched-people" class:empty={matchedPeople.length === 0}>
				{#each matchedPeople as matched}
					<div in:fade>
						<Matched match={matched} />
					</div>
				{:else}
					<div>Henüz çekiliş yapılmadı.</div>
				{/each}
			</div>
		</section>
	</div>

	<a href="https://onderb.dev" class="ob"><img src="/ob.svg" alt="Önder Bakırtaş" /></a>
</main>

{#if !isDrawing && isStarted}
	<div class="confetti">
		<Confetti
			x={[-5, 5]}
			y={[0, 0.1]}
			delay={[50, 3000]}
			duration="1500"
			amount="1000"
			fallDistance="100vh"
		/>
	</div>
{/if}

<style>
	main {
		padding: 1rem;
		display: flex;
		flex-direction: column;
		min-height: 100vh;
	}

	.confetti {
		position: fixed;
		top: -50px;
		left: 0;
		height: 100vh;
		width: 100vw;
		display: flex;
		justify-content: center;
		overflow: hidden;
		pointer-events: none;
	}

	.header {
		text-align: center;
		font-family: 'Codystar', sans-serif;
		margin: 2rem 0 3rem;
		font-size: clamp(2rem, 5vw, 4rem);
		letter-spacing: 0.5rem;
		text-shadow: 0 0 0.5rem #faedf0;
		animation: blink 1s infinite;
	}

	.subheader {
		text-align: center;
		margin: 1rem 0;
		font-size: 1.5rem;
	}

	.matches {
		margin-top: 2rem;
		display: flex;
		flex-direction: column;
	}

	.draw-button {
		display: block;
		margin: 2rem auto 1rem;
		padding: 0.75rem 1.5rem;
		border-radius: 0.5rem;
		background-color: #ec255a;
		color: #faedf0;
		border: none;
		font-size: 1.25rem;
		font-weight: bold;
		cursor: pointer;
		width: 12rem;
		user-select: none;
	}

	.draw-button:disabled {
		background-color: #faedf0;
		color: #ec255a;
		opacity: 0.5;
		cursor: not-allowed;
		pointer-events: none;
	}

	.draw-button:hover {
		background-color: #faedf0;
		color: #ec255a;
	}

	.draw-button:active {
		opacity: 0.8;
	}

	.people {
		display: flex;
		flex-wrap: wrap;
		justify-content: space-between;
		list-style: none;
		margin: 0;
		border-radius: 0.5rem;
		width: 100%;
	}

	.person {
		padding: 0.75rem;
		line-height: 1;
		margin: 0.5rem;
		background-color: #292c6d;
		color: #faedf0;
		border-radius: 0.5rem;
		font-size: 14px;
	}

	.person.drawed {
		text-decoration: line-through;
		opacity: 0.3;
	}

	.person.received {
		color: #ec255a;
	}

	.matches .subheader {
		background-color: #292c6d;
		padding: 0.75rem 1.5rem 0.5rem;
		border-radius: 0.5rem 0.5rem 0 0;
		display: inline-block;
		margin: 0 auto;
		font-size: 1.25rem;
	}

	.matched-people {
		background-color: #292c6d;
		padding: 1rem;
		display: flex;
		flex-direction: row-reverse;
		flex-wrap: wrap;
		justify-content: flex-end;
		border-radius: 0.5rem;
		min-height: 8rem;
		gap: 1rem;
	}

	.matched-people.empty {
		justify-content: center;
		align-items: center;
	}

	.ob {
		display: block;
		margin: auto auto 0.5rem;
		width: 2rem;
		opacity: 0.5;
	}

	.ob:hover {
		opacity: 1;
	}

	.ob img {
		width: 2rem;
	}

	@keyframes blink {
		0% {
			text-shadow: 0 0 0.5rem #faedf0;
		}

		20% {
			text-shadow:
				0 0 0.5rem #faedf0,
				0 0 0.5rem #faedf0;
		}

		30% {
			text-shadow: 0 0 0.5rem #faedf0;
		}

		70% {
			text-shadow: 0 0 0.5rem #faedf0;
		}

		100% {
			text-shadow: 0 0 0.5rem #faedf0;
		}
	}
</style>
