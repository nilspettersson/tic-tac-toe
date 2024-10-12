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
		const checkWin = (a: string, b: string, c: string) => a !== '' && a === b && b === c;
		for (let i = 0; i < 3; i++) {
			if (checkWin(board[i][0], board[i][1], board[i][2])) {
				return { current: board[i][0], won: true };
			} else if (checkWin(board[0][i], board[1][i], board[2][i])) {
				return { current: board[0][i], won: true };
			}
		}

		if (checkWin(board[0][0], board[1][1], board[2][2])) {
			return { current: board[0][0], won: true };
		} else if (checkWin(board[2][0], board[1][1], board[0][2])) {
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
							if (cell !== '' || won().won) return;
							turn === 'X' ? (board[x][y] = 'X') : (board[x][y] = 'O');
							if (turn === 'X') {
								turn = 'O';
							} else {
								turn = 'X';
							}
						}}
						class="aspect-square w-full border-t bg-white text-xl hover:bg-secondary">{cell}</button
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
	<div>
		<p class="py-6 text-2xl font-bold">{turn}</p>
		<button
			onclick={() => {
				turn = 'O';
			}}>asd</button
		>
	</div>
</div>
