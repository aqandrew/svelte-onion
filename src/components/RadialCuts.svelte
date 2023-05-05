<script>
	export let width;
	export let height;
	export let numCuts;
	export let radius;
	export let cutTargetDepthPercentage;
	export let cutScale;
</script>

{#each { length: numCuts } as _, i}
	{@const theta = cutScale(i)}

	<!-- where cut intercepts onion's outermost layer -->
	{@const xIntercept = radius * Math.sin(theta)}
	{@const yIntercept = radius * Math.cos(theta)}

	<line
		x1={width / 2 - xIntercept * 2}
		y1={height - yIntercept * 2 - cutTargetDepthPercentage * height}
		x2={width / 2}
		y2={height + cutTargetDepthPercentage * height}
	/>
{/each}
