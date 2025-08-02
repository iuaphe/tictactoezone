<script lang="ts">
	import { page } from '$app/stores';
	import { backend, wsBackend } from '$lib/config';
	import InteractiveBoard from '$lib/InteractiveBoard.svelte';
	import { onDestroy } from 'svelte';
	import { onNavigate } from '$lib/custom-lifecycle';

	type GameStatus = 'WAITING' | 'ONGOING' | 'ENDED';

	export let board: ('x' | 'o' | '')[][] = [
		['', '', ''],
		['', '', ''],
		['', '', ''],
	];
	export let turn: 'x' | 'o' = 'x';
	export let moves: [number, number][] = [];

	async function move(row: number, column: number) {
		if (board[row][column] !== '') return;
		board[row][column] = turn;
		turn = turn === 'x' ? 'o' : 'x';
		moves = [...moves, [row, column]];
	}

	const convertToNotation = ([row, column]: [number, number]) => {
		return `${['a', 'b', 'c'][column]}${3 - row}`;
	};

	let gameEnded = false;
</script>

<main />

<div class="flex">
	<InteractiveBoard
		bind:gameEnded
		interactive={!gameEnded}
		onclick={move}
		{board}
	/>
	<div class="side moves">
		<div>
			{#each moves as move, i}
				{i % 2 === 0 ? `${Math.floor(i / 2) + 1}. ` : ' '}
				{convertToNotation(move)}
				{#if i % 2 === 1} <br /> {/if}
			{/each}
		</div>
	</div>
</div>
