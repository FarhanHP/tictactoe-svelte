<script>
import Row from "./components/Row.svelte";
let turn = 1;
let tictactoe = [
	[0, 0, 0],
	[0, 0, 0],
	[0, 0, 0]
];

let highlightedCoordinates = [
	[false, false, false],
	[false, false, false],
	[false, false, false]
];

let message = 'X\'s turn';
let disableAll = false;

const copyHighlightedCoordinate = () => {
	return highlightedCoordinates.map((values) => (
			values.map((value) => value)
	));
}

const checkWin = () => {
	/*
		0: continue,
		1: x win,
		2: y win,
		3: draw
	*/
	let isZeroExists = false;

	for(let i=0; i<3; i++) {
		const row = tictactoe[i];
		const newHighlightedCoordinate = copyHighlightedCoordinate();

		for(let j=0; j<3; j++) {
			if (row[j] === 0) {
				isZeroExists = true;
			}

			newHighlightedCoordinate[i][j] = true;
		}

		const stringRow = row.join('');

		//check horizontally
		if(stringRow === '111') {
			highlightedCoordinates = newHighlightedCoordinate;
			return 1;
		} else if (stringRow === '222'){
			highlightedCoordinates = newHighlightedCoordinate;
			return 2;
		}
	}

	//check vertically
	for(let i=0; i<3; i++) {
		let currentCol = '';
		const newHighlightedCoordinate = copyHighlightedCoordinate();

		for(let j=0; j<3; j++) {
			currentCol += String(tictactoe[j][i]);
			newHighlightedCoordinate[j][i] = true;
		}

		if(currentCol === '111') {
			highlightedCoordinates = newHighlightedCoordinate;
			return 1;
		} else if(currentCol === '222') {
			highlightedCoordinates = newHighlightedCoordinate;
			return 2;
		}
	}

	//check diagonally
	for(let i=0; i<2; i++) {
		const newHighlightedCoordinate = copyHighlightedCoordinate();
		let currentDiagonal = '';
		let k = 2;
		for(let j=0; j<3; j++) {
			if(i === 0) {
				currentDiagonal += tictactoe[j][j];
				newHighlightedCoordinate[j][j] = true;
			} else {
				currentDiagonal += tictactoe[j][k];
				newHighlightedCoordinate[j][k] = true;
				k--;
			}
		}

		if(currentDiagonal === '111') {
			highlightedCoordinates = newHighlightedCoordinate;
			return 1;
		} else if (currentDiagonal === '222') {
			highlightedCoordinates = newHighlightedCoordinate;
			return 2;
		}
	}

	return isZeroExists ? 0:3;
}

const handleTileClick = (coordinate) => {
	const [x, y] = coordinate;
	const tictactoeClone = tictactoe.slice();
	tictactoeClone[x][y] = turn;
	tictactoe = tictactoeClone;

	turn = turn === 1 ? 2:1;
	message = turn === 1 ? 'X\'s turn' : 'O\'s turn'

	const status = checkWin();
	if(status === 1) {
		message = 'X\'s Win';
		disableAll = true;
	}	else if(status === 2) {
		message = 'O\'s Win';
		disableAll = true;
	} else if(status === 3) {
		message = 'DRAW';
	}
}

const onRestartClick = () => {
	tictactoe= [
		[0, 0, 0],
		[0, 0, 0],
		[0, 0, 0]
	];
	highlightedCoordinates = [
		[false, false, false],
		[false, false, false],
		[false, false, false]
	];
	message= 'X\'s turn';
	disableAll = false;
}
</script>

<svelte:head>
	<title>TicTacToe Svelte by Farhan</title>
	<meta content="Learn Svelte by making TicTacToe" name="description">
</svelte:head>

<main
	class='root'
>
	<div>
		<h1>Tic-Tac-Toe</h1>
		<p
			class='by'
		>
			By FarhanHP
		</p>
		<p
			class='message'
		>
			{message}
		</p>

		<div
			class='board'
		>
			{#each tictactoe as row, index}
				<Row
					{row}
					rowNumber={index}
					onClickCallback={handleTileClick}
					disabled={disableAll}
					highlightedTiles={highlightedCoordinates[index]}
				/>

				{#if index < row.length-1}
					<div
						class='horizontal-line'
					/>
				{/if}
			{/each}
		</div>

		<div
			class='button-container'
		>
			<button
				class='restart-button'
				on:click={onRestartClick}
			>
				RESTART
			</button>
		</div>
	</div>
</main>

<style>
	:global(*, body) {
		margin: 0;
		padding: 0;
	}

	.by {
		text-align: center;
		color: white;
		margin-bottom: 2rem;
	}

	.button-container {
		display: flex;
		justify-content: center;
		margin-top: 2rem;
	}

	.restart-button {
		padding: 1rem;
		color: white;
		margin: 0;
		background-color: transparent;
		font-weight: bold;
		border: 0.2rem solid white;
	}

	.message {
		color: white;
		text-align: center;
		margin-bottom: 4rem;
	}

	.horizontal-line {
		border-bottom: 0.2rem solid white;
	}

	h1 {
		color: white;
		text-align: center;
	}

	.root {
		min-height: 100vh;
		width: 100%;
		background-color: #263238;
		display: flex;
		justify-content: center;
		align-items: center;
	}

	.board {
		width: 100vw;
		height: 50vh;
		display: flex;
		flex-direction: column;
	}

	@media only screen and (min-width: 600px) {
		.board {
			width: 80vw;
		}
	}
	@media only screen and (min-width: 960px) {
		.board {
			width: 60vw;
		}
	}
	@media only screen and (min-width: 1280px) {
		.board {
			width: 40vw;
			height: 60vh;
		}
	}
	@media only screen and (min-width: 1920px) {
		.board {
			width: 30vw;
			height: 50vh;
		}
	}
</style>