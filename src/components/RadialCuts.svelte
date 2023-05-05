<script>
	export let width;
	export let height;
	export let numCuts;
	export let radius;
	export let cutScale;
</script>

{#each { length: numCuts } as _, i}
	{@const theta = cutScale(i)}

	<!-- TODO extend lines past onion exterior -->
	<line
		x1={width / 2 - radius * Math.sin(theta)}
		y1={height - radius * Math.cos(theta)}
		x2={width / 2}
		y2={height}
	/>
{/each}
