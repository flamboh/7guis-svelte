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

	const t = new Temperature();
</script>

<h1 class="text-jungle m-6 rounded-sm px-2 text-center text-3xl font-bold">Converter!</h1>
<div class="bg-clover border-jungle rounded-sm border-2 p-4">
	<input type="number" bind:value={t.c} class="bg-silver w-[10ch] rounded-sm px-2" />
	Celsius =
	<input type="number" bind:value={t.f} class="bg-silver w-[10ch] rounded-sm px-2" /> Fahrenheit
</div>
