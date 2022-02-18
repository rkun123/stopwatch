<script lang="typescript">
	import Btn from './btn.svelte'
	import IBtn from './stopwatchButton.svelte'
	import '@fortawesome/fontawesome-free/js/fontawesome';
	import '@fortawesome/fontawesome-free/js/solid';
	const tickMS = 10
	export let milliSeconds: number = 0
	export let split: number[] = []
	export let intervalID: number | null = null

	export function reset() {
		if(intervalID !== null) {
			stop()
		}
		milliSeconds = 0
		split = []
		intervalID = null
	}

	export function start(){
		if(intervalID !== null) {
			return
		}
		intervalID = window.setInterval(tick, tickMS)
	}

	export function stop() {
		clearInterval(intervalID)
		intervalID = null
	}

	function tick() {
		milliSeconds += tickMS
	}

	// utils

	export function zp(num: number, degit: number): string {
		if(num >= Math.pow(10, degit-1)) {
			return `${num}`
		}
		let pads = ''
		for(let i=0; i < degit-1; i++) pads = `0${pads}`
		return `${pads}${num}`
	}

	$: running = intervalID !== null
	$: ms = milliSeconds % 1000 / 10
	$: seconds = Math.floor(milliSeconds / 1000 % 60)
	$: minutes = Math.floor(milliSeconds / 1000 / 60 % 60)
	$: hours = Math.floor(milliSeconds / 1000 / 60 / 60)

</script>

<div class="container">
	<main>
		<i class="main-icon fas fa-stopwatch" />
		<div class="time">
			{zp(hours,2)}:{zp(minutes,2)}:{zp(seconds,2)}:{zp(ms,2)}
		</div>
		<div>
			<span class="icon-button" on:click={start} class:disabled={running}>
				<i class="fas fa-play" />
			</span>
			<span class="icon-button" on:click={stop} class:disabled={!running}>
				<i class="fas fa-pause"/>
			</span>
			<span class="icon-button" on:click={reset}>
				<i class="fas fa-rotate-left"/>
			</span>
		</div>
	</main>
</div>

<style lang="scss">
	.container {
		display: flex;
		align-items: center;
		justify-content: center;
		min-height: 100%;
	}
	main {
		text-align: center;
		padding: 1em;
		max-width: 240px;
		margin: 0 auto;
		color: rgb(11, 0, 54);
	}

	.main-icon {
		font-size: 3em;
		margin-bottom: 3rem;
	}

	.time {
		text-transform: uppercase;
		font-size: 4em;
		font-weight: 100;
		font-family: 'Roboto Mono', monospace;
		margin-bottom: 1em;
	}

	@media (min-width: 640px) {
		main {
			max-width: none;
		}
	}

	.icon-button {
		font-size: 2em;
		margin-right: 1em;
		cursor: pointer;
	}
	.disabled {
		color: rgb(188, 188, 188);
		cursor: default;
	}
	.icon-button:last-child {
		margin: 0;
	}
</style>