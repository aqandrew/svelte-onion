<script>
	export let width;
	export let height;
	export let numCuts;
	export let radius;
	export let cutScale;

	let cutTarget = {
		x: width / 2,
		y: height,
	};
</script>

{#each { length: numCuts } as _, i}
	{@const theta = cutScale(i)}

	<!-- where cut intercepts onion's outermost layer -->
	{@const xIntercept = radius * Math.sin(theta)}
	{@const yIntercept = radius * Math.cos(theta)}

	<line
		x1={width / 2 - xIntercept * 2}
		y1={height - yIntercept * 2}
		x2={cutTarget.x}
		y2={cutTarget.y}
	/>
{/each}
