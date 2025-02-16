<script lang="ts">
	import { onMount } from 'svelte';
	import { scale, slide } from 'svelte/transition';
	import src from '$lib/favicon.png';

	const examples = [
		{ label: 'Counter', path: '01_counter' },
		{ label: 'Converter', path: '02_converter' },
		{ label: 'Booker', path: '03_booker' },
		{ label: 'Timer', path: '04_timer' },
		{ label: 'CRUD', path: '05_crud' },
		{ label: 'Circles', path: '06_circles' },
		{ label: 'Cells', path: '07_cells' }
	];

	let ready = false;
	onMount(() => (ready = true));
</script>

<div class="flex flex-col items-center">
	{#if ready}
		<span class="font-body text-4xl" in:scale|global={{ start: 2.5 }}>
			<a
				target="_blank"
				rel="noopener noreferrer"
				href="https://eugenkiss.github.io/7guis/"
				class="underline">7GUIs</a
			> in Svelte</span
		>
		<a href="https://svelte.dev/" target="_blank" rel="noopener noreferrer">
			<img {src} alt="Svelte logo" class="p-6 transition-all hover:scale-120" in:scale|global />
		</a>
		{#each examples as { label, path }, i}
			<a
				href="/examples/{path}"
				class="text-jungle m-2 rounded-sm p-1 text-3xl font-bold transition-all hover:scale-120 hover:animate-pulse"
				in:scale|global={{ delay: i * 20 + 50 }}
			>
				{i + 1}: {label}
			</a>
		{/each}
		<a
			href="/examples/08_connections"
			class="text-jungle m-2 rounded-sm p-1 text-3xl font-bold transition-all hover:scale-120 hover:animate-pulse"
			in:scale|global={{ delay: 7 * 20 + 50 }}
		>
			Bonus: Connections
		</a>
	{/if}
</div>
