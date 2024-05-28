<script lang="ts">
	let turn = $state<'X' | 'O'>('X');
	let board = $state<Array<Array<'X' | 'O' | ''>>>([
		['', '', ''],
		['', '', ''],
		['', '', '']
	]);

	let won = $derived(() => {
		for (let x = 0; x < board.length; x++) {
			let current: 'X' | 'O' | '' = '';
			let found = true;
			for (let y = 0; y < board.length; y++) {
				if (current === '' && board[x][y] !== '') {
					current = board[x][y];
				}
				if (board[x][y] !== current || current === '') {
					found = false;
				}
			}
			if (found) {
				return { current, won: true };
			}
		}

		for (let y = 0; y < board.length; y++) {
			let current: 'X' | 'O' | '' = '';
			let found = true;
			for (let x = 0; x < board.length; x++) {
				if (current === '' && board[x][y] !== '') {
					current = board[x][y];
				}
				if (board[x][y] !== current || current === '') {
					found = false;
				}
			}
			if (found) {
				return { current, won: true };
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

<div class="flex w-full justify-center">
	<h1 class="p-8 text-xl font-bold">Tic Tac Toe</h1>
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
		{won().current} Won
	{/if}
</div>
