<script lang="ts">
	let time = $state(0);
	let timerLength = $state(10);

	let interval: ReturnType<typeof setInterval>;
	function start() {
		interval = setInterval(() => {
			time += 0.01;
			if (time > timerLength) {
				time = timerLength;
				clearInterval(interval);
			}
		}, 10);
	}

	function reset() {
		time = 0;
		clearInterval(interval);
		start();
	}

	$effect(() => {
		if (!timerLength) return;
		start();
		return () => clearInterval(interval);
	});
</script>

<h1 class="text-jungle m-6 rounded-sm px-2 text-center text-3xl font-bold">Timer!</h1>

<div
	class="bg-clover border-jungle flex flex-col items-center justify-center rounded-sm border-2 p-2"
>
	<label>
		<span class="pr-2">Elapsed Time:</span>
		<progress
			value={time}
			max={timerLength}
			class="[&::-webkit-progress-bar]:bg-silver [&::-webkit-progress-value]:bg-moss w-40"
		></progress>
	</label>
	<span class="">
		{Math.round(time * 100) / 100}s
	</span>
	<label>
		<span class="pr-2">Duration:</span>
		<input
			type="range"
			name="timerLength"
			id="timerLength"
			bind:value={timerLength}
			class="w-40"
			max={30}
			step={0.001}
		/>
	</label>
	<button onclick={reset} class="bg-silver w-48 rounded-sm hover:brightness-90"> Reset </button>
</div>
