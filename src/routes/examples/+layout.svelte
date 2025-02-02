<script lang="ts">
	import '../../app.css';
	let { children } = $props();
</script>

<a href="/" class="font-body bg-silver absolute m-20 h-min rounded-sm p-2">Home</a>

{@render children()}
