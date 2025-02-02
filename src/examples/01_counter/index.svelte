<script lang="ts">
	let count = $state(0);
</script>

<div class="bg-keppel font-body flex h-screen">
	<div class="bg-mint m-16 flex w-full flex-col items-center justify-center space-x-2 rounded-4xl">
		<h1 class="text-jungle m-6 rounded-sm px-2 text-center text-3xl font-bold">Counter!</h1>
		<div class="bg-moss flex items-center justify-center space-x-2 rounded-sm p-4">
			<input type="number" bind:value={count} disabled class="bg-clover w-max rounded-sm p-2" />
			<button onclick={() => count++} class="bg-silver rounded-sm p-2 hover:brightness-90"
				>Count</button
			>
		</div>
	</div>
</div>
