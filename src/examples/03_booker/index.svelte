<script lang="ts">
	type Options = 'one-way' | 'return';

	let today = new Date().toJSON().slice(0, 10);
	let startDate = $state(today);
	let returnDate = $state(today);

	let selected = $state<Options>('one-way');

	function handleSubmit(e: Event) {
		e.preventDefault();
		if (selected === 'one-way') {
			alert(`Booked a ${selected} flight on ${startDate}`);
		} else {
			alert(`Booked a ${selected} flight on ${startDate} with a return on ${returnDate}`);
		}
	}

	function validateFlight() {
		if (selected === 'return' && startDate <= returnDate) {
			return true;
		} else if (selected === 'one-way') {
			return true;
		}
		return false;
	}

	$inspect({ selected, startDate, returnDate });
</script>

<h1 class="text-jungle m-6 rounded-sm px-2 text-center text-3xl font-bold">Booker!</h1>

<form
	class="bg-clover border-jungle flex flex-col items-center justify-center rounded-sm border-2 p-2"
	onsubmit={handleSubmit}
>
	<select
		class="bg-silver m-2 w-48 rounded-sm"
		id="flights"
		bind:value={selected}
		placeholder={selected}
	>
		<option value="one-way">one-way flight</option>
		<option value="return">return flight</option>
	</select>
	<label>
		<span class="hidden">Select the start date:</span>
		<input class="bg-silver m-1 w-48" type="date" bind:value={startDate} />
	</label>
	<label>
		<span class="hidden">Select the return date:</span>
		<input
			class="bg-silver m-1 w-48 disabled:brightness-50"
			disabled={selected === 'one-way'}
			type="date"
			bind:value={returnDate}
		/>
	</label>
	<button
		class="bg-silver m-2 w-48 rounded-sm hover:brightness-90 disabled:brightness-50"
		disabled={!validateFlight()}
		type="submit">Book</button
	>
</form>

<!-- <div class="bg-clover flex flex-col items-center rounded-sm p-2">
	<select class="bg-silver m-2 w-48 rounded-sm" id="flights" bind:value={flights}>
		<option value="one">one-way flight</option>
		<option value="round">return flight</option>
	</select>
	<input class="bg-silver m-1 w-48" type="date" bind:value={flight1} />
	<input
		class="bg-silver m-1 w-48 disabled:brightness-50"
		disabled={flights === 'one'}
		type="date"
		bind:value={flight2}
	/>
	<button
		class="bg-silver m-2 w-48 rounded-sm hover:brightness-90 disabled:brightness-50"
		disabled={!validateFlight()}
		onclick={() => alert('Flight booked!')}>book</button
	>
</div> -->
