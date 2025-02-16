<script lang="ts">
	import { scale } from 'svelte/transition';
	import { page } from '$app/stores';
	import '../app.css';
	let { children } = $props();
</script>

{#if $page.url.pathname.includes('examples')}
	<a
		href="/"
		class="font-body bg-silver absolute m-10 h-min rounded-sm p-2 lg:m-20"
		transition:scale>Home</a
	>
{/if}

<div class="bg-keppel font-body flex h-screen">
	<div
		class="bg-mint border-jungle m-4 flex w-full flex-col items-center justify-center rounded-4xl border-2 lg:m-16"
	>
		{@render children()}
	</div>
</div>
