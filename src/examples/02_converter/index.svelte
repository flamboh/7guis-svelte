<script lang="ts">
	class Temperature {
		#c = $state(0);
		#f = $state(0);

		get c() {
			return this.#c;
		}

		set c(c: number) {
			this.#c = c;
			this.#f = c * (9 / 5) + 32;
		}

		get f() {
			return this.#f;
		}

		set f(f: number) {
			this.#f = f;
			this.#c = (f - 32) * (5 / 9);
		}

		constructor() {
			this.#c = 0;
			this.#f = 32;
		}
	}
	const T = new Temperature();
</script>

<div class="bg-keppel font-body flex h-screen">
	<div
		class="font-body bg-mint m-16 flex w-full flex-col items-center justify-center space-x-2 rounded-4xl"
	>
		<h1 class="text-jungle m-6 rounded-sm px-2 text-center text-3xl font-bold">Converter!</h1>
		<div class="bg-moss text-silver rounded-sm p-4">
			<input type="number" bind:value={T.c} class="bg-clover w-[10ch] rounded-sm px-2" />
			Celsius =
			<input type="number" bind:value={T.f} class="bg-clover w-[10ch] rounded-sm px-2" /> Fahrenheit
		</div>
	</div>
</div>
