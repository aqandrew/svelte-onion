<script>
	export let width;
	export let height;
	export let numLayers;
	export let rScale;
</script>

<g class="onion">
	{#each { length: numLayers } as _, i}
		<circle cx={width / 2} cy={height} r={rScale(i)} />
	{/each}
</g>

<style>
	circle {
		fill: none;
		stroke: black;
	}
</style>
