<script lang="ts">
	type circle = {
		x: number;
		y: number;
		id: number;
		r: number;
	};

	let circles: Array<circle> = [];

	let m = $state({ x: 0, y: 0 });

  function drawCircle() {
      
  }
</script>

<div
	class="bg-clover border-jungle flex h-[70vh] w-[70vw] flex-col items-center justify-center rounded-sm border-2 p-2"
	onpointermove={(e) => {
		m.x = e.clientX;
		m.y = e.clientY;
	}}
>
	x: {m.x.toFixed(2)} y: {m.y.toFixed(2)}
	<svg viewBox="0 0 600 400" class="bg-red-400"
  onclick={drawCircle}></svg>
</div>
