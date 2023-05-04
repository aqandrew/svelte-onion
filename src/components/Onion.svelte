<script>
	import { scaleLinear } from 'd3-scale';

	export let width;
	export let height;
	export let numLayers;

	const maxRadiusProportion = 0.8; // proportional to graph height
	let maxRadius = height * maxRadiusProportion;
	let minRadius = maxRadius / numLayers;

	const rScale = scaleLinear()
		.domain([0, numLayers])
		.range([minRadius, maxRadius]);
</script>

{#each { length: numLayers } as _, i}
	<circle cx={width / 2} cy={height} r={rScale(i)} />
{/each}

<style>
	circle {
		fill: none;
		stroke: black;
	}
</style>
