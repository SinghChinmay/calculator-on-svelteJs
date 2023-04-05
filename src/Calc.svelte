<script lang="ts">
	import { onMount } from 'svelte';

	let display = '';

	const randomHexColor = () => {
		const hex = Math.floor(Math.random() * 0xffffff).toString(16);
		return `#${hex.padStart(6, '0')}`;
	};

	const OppositeHexColor = (hex: string) => {
		const hexColor = hex.replace('#', '');
		const r = (255 - parseInt(hexColor.substr(0, 2), 16)).toString(16).padStart(2, '0');
		const g = (255 - parseInt(hexColor.substr(2, 2), 16)).toString(16).padStart(2, '0');
		const b = (255 - parseInt(hexColor.substr(4, 2), 16)).toString(16).padStart(2, '0');
		return `#${r}${g}${b}`;
	};

	const buttonColors: { [key: string]: string } = {
		'7': randomHexColor(),
		'8': randomHexColor(),
		'9': randomHexColor(),
		'/': randomHexColor(),
		'4': randomHexColor(),
		'5': randomHexColor(),
		'6': randomHexColor(),
		'*': randomHexColor(),
		'1': randomHexColor(),
		'2': randomHexColor(),
		'3': randomHexColor(),
		'-': randomHexColor(),
		'0': randomHexColor(),
		'.': randomHexColor(),
		'=': randomHexColor(),
		'+': randomHexColor(),
		C: randomHexColor()
	};

	const textColors: { [key: string]: string } = {
		'7': OppositeHexColor(buttonColors['7']),
		'8': OppositeHexColor(buttonColors['8']),
		'9': OppositeHexColor(buttonColors['9']),
		'/': OppositeHexColor(buttonColors['/']),
		'4': OppositeHexColor(buttonColors['4']),
		'5': OppositeHexColor(buttonColors['5']),
		'6': OppositeHexColor(buttonColors['6']),
		'*': OppositeHexColor(buttonColors['*']),
		'1': OppositeHexColor(buttonColors['1']),
		'2': OppositeHexColor(buttonColors['2']),
		'3': OppositeHexColor(buttonColors['3']),
		'-': OppositeHexColor(buttonColors['-']),
		'0': OppositeHexColor(buttonColors['0']),
		'.': OppositeHexColor(buttonColors['.']),
		'=': OppositeHexColor(buttonColors['=']),
		'+': OppositeHexColor(buttonColors['+']),
		C: OppositeHexColor(buttonColors['C'])
	};

	const buttonsArr = [
		'7',
		'8',
		'9',
		'/',
		'4',
		'5',
		'6',
		'*',
		'1',
		'2',
		'3',
		'-',
		'0',
		'.',
		'=',
		'+',
		'C'
	];

	onMount(() => {
		display = '';
		getTime();
	});

	const displayEval = () => {
		// check if last character is an operator
		if (['+', '-', '*', '/'].includes(display[display.length - 1])) {
			display = display.slice(0, -1);
			console.log(display);
		}

		// check if it only contains numbers and operators
		if (!display.match(/^[0-9+\-*/.]+$/)) {
			display = 'Invalid input.. reseting in 3';
			setTimeout(() => {
				display = 'Invalid input.. reseting in 2';
				setTimeout(() => {
					display = 'Invalid input.. reseting in 1';
					setTimeout(() => {
						display = '';
					}, 1000);
				}, 1000);
			}, 1000);

			return;
		}

		display = eval(display);
	};

	let time: string = 'Loading...';

	const getTime = () => {
		const date = new Date();
		const hours = date.getHours().toString().padStart(2, '0');
		const minutes = date.getMinutes().toString().padStart(2, '0');
		const seconds = date.getSeconds().toString().padStart(2, '0');
		time = `${hours}:${minutes}:${seconds}`;
	};

	setInterval(getTime, 1000);
</script>

<div class="displayTime">
	{#if time}
		<h1>{time}</h1>
	{/if}
</div>
<div class="calculator">
	<input type="text" class="display" readonly value={display} />
	<div class="buttons">
		{#each buttonsArr as button}
			{#if button === 'C'}
				<button
					class="clear"
					on:click={() => (display = '')}
					style="background-color: {buttonColors[button]};  color: {textColors[button]};"
				>
					{button}
				</button>
			{:else if button === '='}
				<button
					on:click={() => displayEval()}
					style="background-color: {buttonColors[button]};  color: {textColors[button]};"
				>
					{button}
				</button>
			{:else}
				<button
					on:click={() => (display += button)}
					style="background-color: {buttonColors[button]};  color: {textColors[button]};"
					>{button}</button
				>
			{/if}
		{/each}
	</div>
</div>

<style>
	.displayTime {
		display: flex;
		justify-content: center;
		align-items: center;
		flex: 0.4;
	}

	.calculator {
		border: 1px solid #ccc;
		border-radius: 5px;
		padding: 1rem;
		max-width: 400px;
		margin: 100px auto;
	}

	.display {
		width: 100%;
		margin-bottom: 10px;
		text-align: right;
		padding: 5px;
		font-size: 1.5em;
		box-sizing: border-box;
	}

	.buttons {
		display: grid;
		grid-template-columns: repeat(4, 1fr);
		gap: 5px;
	}

	button {
		font-size: 1.5em;
		padding: 5px;
		background-color: #ccc;
		border: none;
		border-radius: 5px;
		cursor: pointer;
	}

	button:hover {
		background-color: #ddd;
	}

	button.clear {
		grid-column: 1 / span 4;
		background-color: rgb(255, 57, 57);
		color: #fff;
	}

	button.clear:hover {
		background-color: #f99;
	}
</style>
