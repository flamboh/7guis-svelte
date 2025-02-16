<script lang="ts">
	const colors: Record<string, string> = {
		yellow: 'bg-yellow-500',
		green: 'bg-green-500',
		blue: 'bg-blue-500',
		purple: 'bg-purple-500'
	};

	type category = {
		description: string;
		items: string[];
		color: string;
	};
	type gridItem = {
		description: string;
		item: string;
		selected: boolean;
	};

	let correctGuesses: Array<category> = $state([
		// {
		// 	description: 'things a rattlesnake does',
		// 	items: ['hiss', 'rattle', 'shed', 'slither'],
		// 	color: 'green'
		// }
	]);

	let categories: Array<category> = [
		{
			description: 'last.fm top 4 (excluding matt maltese)',
			items: ['mitski', 'fiona', 'elliot', 'tyler'],
			color: 'yellow'
		},
		{
			description: 'r/battlebuildsurvive intro answers',
			items: ['they/them', '2005', 'music tech', 'sophia'],
			color: 'green'
		},
		{
			description: 'artists seen live 2024',
			items: ['poison', 'jane', 'julie', 'origami'],
			color: 'blue'
		},
		{
			description: 'letterboxd top 4 protagonists',
			items: ['marianne', 'scooby', 'neil', 'donnie'],
			color: 'purple'
		}
	];

	let gridCategories: Array<gridItem> = $state([]);
	let startPointer = $state(0);
	let numSelected = $state(0);
	let guesses = $state(0);
	let won = $derived(correctGuesses.length == 4);

	for (const category of categories) {
		for (const item of category.items) {
			gridCategories.push({
				description: category.description,
				item: item,
				selected: false
			});
		}
	}

	shuffle(null, gridCategories);

	function select(e: Event, gridItem: gridItem) {
		e.preventDefault();
		if (gridItem.selected) {
			gridItem.selected = !gridItem.selected;
			numSelected -= 1;
		} else if (numSelected < 4) {
			gridItem.selected = !gridItem.selected;
			numSelected += 1;
		}
	}

	function deselectAll(e: Event) {
		e.preventDefault();
		for (const gridItem of gridCategories) {
			gridItem.selected = false;
		}
		numSelected = 0;
	}
	function shuffle(e: Event | null, array: Array<gridItem>) {
		if (e) {
			e.preventDefault();
		}
		for (let i = array.length - 1; i > startPointer; i--) {
			const j = Math.floor(Math.random() * (i - startPointer + 1) + startPointer); // Generate random index from 0 to i
			[array[i], array[j]] = [array[j], array[i]]; // Swap elements at i and j
		}
		return array;
	}

	function submit(e: Event) {
		e.preventDefault();
		let selectedItems: Array<[gridItem, number]> = [];
		let correctCategory: string | null = null;
		for (let i = startPointer; i < 16; i++) {
			if (gridCategories[i].selected) {
				if (!correctCategory) {
					correctCategory = gridCategories[i].description;
				}
				selectedItems.push([gridCategories[i], i]);
			}
		}
		let correctItems: number = 0;
		for (const item of selectedItems) {
			if (item[0].description == correctCategory) {
				correctItems++;
			}
		}
		console.log(correctItems);
		if (correctItems == 4) {
			console.log(selectedItems);
			for (let i = 0; i < 4; i++) {
				let temp = gridCategories[startPointer];
				gridCategories[startPointer++] = gridCategories[selectedItems[i][1]];
				console.log(selectedItems[i][1]);
				gridCategories[selectedItems[i][1]] = temp;
			}
			for (const category of categories) {
				if (correctCategory == category.description) {
					correctGuesses.push(category);
				}
			}
			deselectAll(e);
		} else {
			guesses++;
			if (guesses > 3) {
				deselectAll(e);
			}
			alert('Try again!');
		}
	}
</script>

<svg height="30" width="200" xmlns="http://www.w3.org/2000/svg">
	<text x="5" y="15" fill="none" class="text-9xl font-bold backdrop-hue-rotate-90">I love SVG!</text
	>
</svg>

<h1 class="text-jungle m-6 rounded-sm px-2 text-center text-3xl font-bold">Connections!</h1>

<div class="mb-2 flex space-x-2">
	<div
		class={'my-2 rounded-full bg-white transition-all' + (guesses > 3 ? ' size-0' : ' size-6')}
	></div>
	<div
		class={'my-2 rounded-full bg-white transition-all' + (guesses > 2 ? ' size-0' : ' size-6')}
	></div>
	<div
		class={'my-2 rounded-full bg-white transition-all' + (guesses > 1 ? ' size-0' : ' size-6')}
	></div>
	<div
		class={'my-2 rounded-full bg-white transition-all' + (guesses > 0 ? ' size-0' : ' size-6')}
	></div>
</div>

<div
	class="bg-clover border-jungle flex flex-col items-center justify-center rounded-sm border-2 p-2"
>
	<form action="" class="flex flex-col items-center justify-center">
		<div
			class=" grid grid-cols-[repeat(4,85px)] grid-rows-[repeat(4,60px)] items-center justify-center text-[10px] lg:grid-cols-[repeat(4,150px)] lg:grid-rows-[repeat(4,100px)] lg:text-[16px]"
		>
			{#each correctGuesses as correct}
				<div class={'col-span-4 m-1 rounded-sm p-1 text-center ' + colors[correct.color]}>
					<h1 class="font-semibold">
						{correct.description.toUpperCase()}
					</h1>
					{correct.items.join(', ').toUpperCase()}
				</div>
			{/each}
			{#each gridCategories as gridItem, i}
				<!-- if the index is greater than a start pointer 
        display guesses
        -->
				{#if i >= startPointer}
					<button
						class={'bg-silver m-0.5 rounded-sm py-4 text-center font-semibold transition-all disabled:hover:cursor-default lg:m-1 lg:p-5 lg:py-8' +
							(gridItem.selected ? ' brightness-70 hover:cursor-pointer' : ' ') +
							(numSelected < 4 ? ' hover:cursor-pointer' : '')}
						onclick={(e) => select(e, gridItem)}
						disabled={guesses > 3}
					>
						<span>
							{gridItem.item.toUpperCase()}
						</span>
					</button>
				{/if}
			{/each}
		</div>
		<div class="space-x-4 p-2">
			<button
				class="bg-silver rounded-4xl p-1 px-2 hover:cursor-pointer"
				onclick={(e) => {
					shuffle(e, gridCategories);
				}}
				disabled={guesses > 3}
				>Shuffle
			</button>
			<button
				class="bg-silver rounded-4xl p-1 px-2 enabled:hover:cursor-pointer disabled:brightness-50"
				onclick={(e) => deselectAll(e)}
				disabled={numSelected < 1 || guesses > 3}
				>Deselect all
			</button>
			<button
				class="bg-silver rounded-4xl p-1 px-2 enabled:hover:cursor-pointer disabled:brightness-50"
				onclick={(e) => submit(e)}
				disabled={numSelected != 4 || guesses > 3}
				>Submit
			</button>
		</div>
	</form>
</div>

{#if won}
	<h1
		class=" pointer-events-none absolute inset-0 flex animate-bounce items-center justify-center pt-64 text-5xl font-bold"
	>
		You won!!
	</h1>
{/if}

{#if guesses > 3}
	<h1
		class=" pointer-events-none absolute inset-0 flex animate-bounce items-center justify-center pt-64 text-5xl font-bold"
	>
		{'You lost :(('}
	</h1>
{/if}
