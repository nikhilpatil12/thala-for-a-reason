<script lang="ts">
	import { Label, Input, Button, Video } from 'flowbite-svelte';
	let showThala = false;
	let showReason = false;
	let song: HTMLAudioElement | null = null;
	const thala_ka_reason_dede = (number: string) => {
		let digits = Array.from(String(number), Number);
		let digit2 = 0;
		let add = true;
		if (digits.reduce((sum, digit) => sum + digit, 0) <= 7) {
			digit2 = 7 - digits.reduce((sum, digit) => sum + digit, 0);
			add = true;
		} else {
			digit2 = digits.reduce((sum, digit) => sum + digit, 0) - 7;
			add = false;
		}

		let formula = digits.map((digit) => `${digit}`).join(' + ');

		if (digit2 !== 0) {
			if (add) {
				formula += ' + ';
			} else {
				formula += ' - ';
			}

			formula += `${digit2}`;
		}
		formula += ` = 7`;

		return formula;
	};
	var formulaUsed = '';
	var inputNumber = '';
	const generateFormula = () => {
		if (!isNaN(Number(inputNumber))) {
			showThala = false;
			showReason = true;
			const formula = thala_ka_reason_dede(inputNumber);
			formulaUsed = formula;
			song = new Audio('/surprise.mp3');
			song.volume = 1;
			song.play();
		} else {
			showAngryThala();
		}
	};
	const showAngryThala = () => {
		console.log('wtf');
		if (isNaN(Number(inputNumber))) {
			if (song && showReason) {
				song.pause();
			}
			formulaUsed = 'Enter a number 👹';
			showThala = true;
			showReason = false;
		} else {
			formulaUsed = '';
			showThala = false;
			showReason = false;
		}
	};
</script>

<div class="{showReason ? 'yellowlove' : ''} mb-6 h-screen p-12 font-mono">
	<p class="p-8 text-4xl md:text-9xl dark:text-white">Why is he Thala?🏏🦁</p>
	<Label for="input-group-1" class="mb-2 block">Enter a number</Label>
	<Input
		on:keyup={showAngryThala}
		bind:value={inputNumber}
		id="number"
		type="text"
		placeholder="Thala Ka Reason"
	></Input>
	<Button class="my-8 w-full " color="yellow" on:click={generateFormula}>Get Reason 🏏🦁</Button>
	{#if showReason}
		<p class="text-4xl md:text-9xl dark:text-white">{formulaUsed}🏏🦁</p>
		<p class="text-3xl md:text-6xl dark:text-white">Thala for a Reason!💛🏏🦁</p>
	{/if}
	{#if showThala}
		<Video class="w-200 m-auto" src="/thala.mp4" autoplay controls></Video>
	{/if}
</div>
