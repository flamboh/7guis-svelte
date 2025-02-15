<script lang="ts">
	import { json } from '@sveltejs/kit';

	type person = {
		first_name: string;
		last_name: string;
	};
	let people = $state<person[]>([]);

	let name = $state('');
	let surname = $state('');

	let filter = $state('');
	let selected = $state(-1);

	function handleSubmit(e: Event) {
		e.preventDefault();
		if (!name) {
			alert('Please add a name.');
			return;
		} else if (!surname) {
			alert('Please add a surname.');
			return;
		}

		people = [
			...people,
			{
				first_name: name,
				last_name: surname
			}
		];
	}

	function handleDelete() {
		if (selected == -1) return;
		people.splice(selected, 1);
		selected = -1;
		// let toRemove: Array<number> = selected;
		// toRemove.sort((a, b) => b - a); // sorted descending so that removing items doesn't change array indexes

		// for (const index of toRemove) {
		// 	if (index >= 0 && index < people.length) {
		// 		people.splice(index, 1);
		// 	}
		// }
		// selected = [];
	}

	function handleUpdate() {
		if (!name) {
			alert('Please add a name.');
			return;
		} else if (!surname) {
			alert('Please add a surname.');
			return;
		}
		if (selected == -1) return;
		people[selected] = {
			first_name: name,
			last_name: surname
		};
		selected = -1;
		// for (const index of selected) {
		// 	if (index >= 0 && index < people.length) {
		// 		people[index] = {
		// 			first_name: name,
		// 			last_name: surname
		// 		};
		// 	}
		// }
	}

	$effect(() => {
		name = people[selected]?.first_name ?? '';
		surname = people[selected]?.last_name ?? '';
	});

	$effect(() => {
		const savedPeople = localStorage.getItem('people');
		if (savedPeople) {
			people = JSON.parse(savedPeople);
		}
	});

	$effect(() => {
		localStorage.setItem('people', JSON.stringify(people));
	});
</script>

<h1 class="text-jungle m-6 rounded-sm px-2 text-center text-3xl font-bold">CRUD!</h1>

<div
	class="bg-clover border-jungle flex flex-col items-center justify-center rounded-sm border-2 p-2"
>
	<form
		class="grid grid-cols-[224px_224px] grid-rows-[48px_124px_48px] justify-center space-y-2 space-x-2"
		onsubmit={handleSubmit}
	>
		<div class="row-start-1 w-[95%] self-center">
			<label class=" flex justify-between">
				<span>Filter prefix: </span>
				<input
					type="text"
					class="bg-silver border-jungle w-20 rounded-sm border-2 pl-0.5"
					bind:value={filter}
				/>
			</label>
		</div>
		<div class="bg-silver border-jungle row-start-2 w-[95%] overflow-hidden border-2">
			<select size={10} class="h-full w-full pt-0.5 pl-0.5" bind:value={selected}>
				{#each people as person, i}
					{#if person.last_name.toLowerCase().startsWith(filter.toLowerCase())}
						<option value={i}>
							{person.last_name}, {person.first_name}
						</option>
					{/if}
				{/each}
			</select>
		</div>
		<div class="col-start-2 row-start-2 flex flex-col space-y-1">
			<label class="flex justify-between">
				<span>Name:</span>
				<input
					type="text"
					class="bg-silver border-jungle ml-4 w-[50%] rounded-sm border-2 pl-0.5"
					bind:value={name}
				/>
			</label>
			<label class="flex justify-between">
				<span>Surname:</span>
				<input
					type="text"
					class="bg-silver border-jungle ml-4 w-[50%] rounded-sm border-2 pl-0.5"
					bind:value={surname}
				/>
			</label>
		</div>
		<div class="col-span-2 row-start-3 space-x-4 self-center justify-self-center">
			<button class="bg-silver rounded-sm p-1 hover:brightness-90" type="submit">Create</button>
			<button
				class="bg-silver rounded-sm p-1 hover:brightness-90 disabled:brightness-50"
				disabled={selected == -1}
				type="button"
				onclick={handleUpdate}>Update</button
			>
			<button
				class="bg-silver rounded-sm p-1 hover:brightness-90 disabled:brightness-50"
				disabled={selected == -1}
				type="button"
				onclick={handleDelete}>Delete</button
			>
		</div>
	</form>
</div>
