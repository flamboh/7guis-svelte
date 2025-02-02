<script lang="ts">
	const examples = [
		{ label: 'Counter', path: '01_counter' },
		{ label: 'Converter', path: '02_converter' }
	];
</script>

<div class="bg-keppel font-body flex h-screen">
	<div class="bg-mint m-16 flex w-full flex-col items-center justify-center rounded-4xl">
		{#each examples as { label, path }, i}
			<a href="/examples/{path}" class="text-jungle m-2 rounded-sm p-1 text-3xl font-bold">
				{i + 1}: {label}
			</a>
		{/each}
	</div>
</div>
