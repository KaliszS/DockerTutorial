<script lang="ts">
	import BookList from "./components/BookList.svelte";
	import Chapter1 from "./components/chapters/Chapter1.svelte";
	import Chapter2 from "./components/chapters/Chapter2.svelte";
	import Chapter3 from "./components/chapters/Chapter3.svelte";
	import Chapter4 from "./components/chapters/Chapter4.svelte";
	import Chapter5 from "./components/chapters/Chapter5.svelte";
	import Chapter6 from "./components/chapters/Chapter6.svelte";
	import Chapter7 from "./components/chapters/Chapter7.svelte";
	import Chapter8 from "./components/chapters/Chapter8.svelte";

	let items = [
		{
			id: 1,
			title: "Rozdział 1",
			subtitle: "Konteneryzacja",
			content: "Na czym polega konteneryzacja?",
			bg: "1.png",
			h: 20,
		},
		{
			id: 2,
			title: "Rozdział 2",
			subtitle: "Obraz vs kontener",
			content: "Czym się różnią obrazy od kontenerów?",
			bg: "2.jpg",
			h: 60,
		},
		{
			id: 3,
			title: "Rozdział 3",
			subtitle: "Docker Hub",
			content: "O największym repozytorium obrazów dockerowych.",
			bg: "3.png",
			h: 100,
		},
		{
			id: 4,
			title: "Rozdział 4",
			subtitle: "Dockerfile",
			content: "Jak tworzyć własne obrazy i budować z nich kontenery?",
			bg: "4.jpg",
			h: 140,
		},
		{
			id: 5,
			title: "Rozdział 5",
			subtitle: "Docker compose",
			content: "Czyli aplikacje wielokontenerowe.",
			bg: "5.png",
			h: 180,
		},
		{
			id: 6,
			title: "Rozdział 6",
			subtitle: "Sieci kontenerów",
			content: "Jak komunikują się kontenery?",
			bg: "6.png",
			h: 200,
		},
		{
			id: 7,
			title: "Rozdział 7",
			subtitle: "Woluminy",
			content: "O współdzieleniu zasobów.",
			bg: "7.png",
			h: 240,
		},
		{
			id: 8,
			title: "Rozdział 8",
			subtitle: "Dockerowe tricki",
			content:
				"Uzupełniający rozdział o bardziej zaawansowanych wskazówkach.",
			bg: "8.png",
			h: 290,
		},
	];

	let switchers = {
		menu: true,
		ch1: false,
		ch2: false,
		ch3: false,
		ch4: false,
		ch5: false,
		ch6: false,
		ch7: false,
		ch8: false,
	};
</script>

<main>
	{#if switchers.menu}
		<section class="subbody">
			<BookList {items} bind:switchers />
		</section>
	{:else if switchers.ch1}
		<Chapter1 bind:switchers />
	{:else if switchers.ch2}
		<Chapter2 bind:switchers />
	{:else if switchers.ch3}
		<Chapter3 bind:switchers />
	{:else if switchers.ch4}
		<Chapter4 bind:switchers />
	{:else if switchers.ch5}
		<Chapter5 bind:switchers />
	{:else if switchers.ch6}
		<Chapter6 bind:switchers />
	{:else if switchers.ch7}
		<Chapter7 bind:switchers />
	{:else if switchers.ch8}
		<Chapter8 bind:switchers />
	{/if}
</main>

<style>
</style>
