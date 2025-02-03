<script lang="ts">
	import { scale } from 'svelte/transition';

	let count = $state(0);
</script>

<div class="flex flex-col items-center" transition:scale|global>
	<h1 class="text-jungle m-6 rounded-sm px-2 text-center text-3xl font-bold">Counter!</h1>
	<div
		class=" bg-clover border-jungle flex items-center justify-center space-x-2 rounded-sm border-2 p-4"
	>
		<input type="number" bind:value={count} disabled class="bg-silver max-w-max rounded-sm p-2" />
		<button onclick={() => count++} class="bg-silver rounded-sm p-2 hover:brightness-90"
			>Count</button
		>
	</div>
</div>
