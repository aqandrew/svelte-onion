<script>
	import { scaleLinear } from 'd3-scale';

	export let cutType;
	export let numCuts;
	export let width;
	export let height;
	export let radius;
	export let cutTargetDepthPercentage;

	$: cutWidthScale = scaleLinear()
		.domain([0, numCuts])
		.range([width / 2, width / 2 - radius]);

	$: cutAngleScale = scaleLinear()
		.domain([0, numCuts])
		.range([0, Math.PI / 2]);
</script>

<g class="cuts">
	{#each { length: numCuts } as _, i}
		{#if cutType === 'vertical'}
			{@const x = cutWidthScale(i)}

			<line x1={x} y1="0" x2={x} y2={height} />
		{:else if cutType === 'radial'}
			{@const theta = cutAngleScale(i)}

			<!-- where cut intercepts onion's outermost layer -->
			{@const xIntercept = radius * Math.sin(theta)}
			{@const yIntercept = radius * Math.cos(theta)}

			<line
				x1={width / 2 - xIntercept * 2}
				y1={height - yIntercept * 2 - cutTargetDepthPercentage * height}
				x2={width / 2}
				y2={height + cutTargetDepthPercentage * height}
			/>
		{/if}
	{/each}
</g>
