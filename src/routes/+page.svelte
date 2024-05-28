<script lang="ts">
	import { Button } from '$lib/components/ui/button';

	const initalBoard = (): Array<Array<'X' | 'O' | ''>> => [
		['', '', ''],
		['', '', ''],
		['', '', '']
	];

	let turn = $state<'X' | 'O'>('X');
	let board = $state<Array<Array<'X' | 'O' | ''>>>(initalBoard());

	let won = $derived(() => {
		for (let x = 0; x < board.length; x++) {
			if (board[x][0] === '') continue;
			if (board[x][0] === board[x][1] && board[x][1] === board[x][2]) {
				return { current: board[x][0], won: true };
			}
		}
		for (let y = 0; y < board.length; y++) {
			if (board[0][y] === '') continue;
			if (board[0][y] === board[1][y] && board[1][y] === board[2][y]) {
				return { current: board[0][y], won: true };
			}
		}

		if (board[0][0] !== '' && board[0][0] === board[1][1] && board[1][1] === board[2][2]) {
			return { current: board[0][0], won: true };
		}
		if (board[2][0] !== '' && board[2][0] === board[1][1] && board[1][1] === board[0][2]) {
			return { current: board[2][0], won: true };
		}

		return { current: '', won: false };
	});
</script>

<div class="flex w-full flex-col items-center">
	<div class="flex items-center gap-4 p-8">
		<h1 class=" text-xl font-bold">Tic Tac Toe</h1>
		<Button onclick={() => (board = initalBoard())} variant="outline">Reset</Button>
	</div>

	<div class="m-auto grid aspect-square w-1/2 max-w-[30rem] grid-rows-3 border">
		{#each board as row, x}
			<div class="grid grid-cols-3 divide-x">
				{#each row as cell, y}
					<button
						onclick={() => {
							if (cell !== '') return;
							turn === 'X' ? (board[x][y] = 'X') : (board[x][y] = 'O');
							if (turn === 'X') {
								turn = 'O';
							} else {
								turn = 'X';
							}
						}}
						class="aspect-square w-full border-t bg-white hover:bg-secondary">{cell}</button
					>
				{/each}
			</div>
		{/each}
	</div>
	<div>
		{#if won().won}
			<p class="py-6 text-2xl font-bold text-green-500">{won().current} Won</p>
		{/if}
	</div>
</div>
