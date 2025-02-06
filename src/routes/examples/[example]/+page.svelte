<script lang="ts">
	import { scale } from 'svelte/transition';
	import { page } from '$app/stores';

	const example = $derived(import(`../../../examples/${$page.params.example}/index.svelte`));
</script>

{#await example then component}
	<div class="flex flex-col items-center" in:scale|global={{ duration: 100 }}>
		<component.default />
	</div>
{/await}
