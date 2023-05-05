<script>
	import { scaleLinear } from 'd3-scale';
	import { format } from 'd3-format';
	import AxisX from './components/AxisX.svelte';
	import AxisY from './components/AxisY.svelte';
	import Onion from './components/Onion.svelte';
	import Cuts from './components/Cuts.svelte';

	let width = 600;
	let height = width / 2;
	let numLayers = 10;
	let maxCuts = numLayers;
	let numCuts = maxCuts;
	let cutType = 'radial';
	let cutTargetDepthPercentage = 0;

	const radiusPercentage = 0.8; // proportional to graph height
	const radius = height * radiusPercentage;

	const rScale = scaleLinear().domain([0, numLayers]).range([0, radius]);

	function formatPercentageAsNegative(n) {
		return format('.0%')(-n);
	}
</script>

<svg {width} {height}>
	<AxisX {width} {height} />
	<AxisX {width} {height} isBottom />
	<!-- TODO responsive sizing: move y axis when screen resizes -->
	<AxisY {width} {height} />

	<Onion {width} {height} {numLayers} {rScale} />

	<Cuts
		{cutType}
		{numCuts}
		{width}
		{height}
		{radius}
		{cutTargetDepthPercentage}
	/>
</svg>

<div class="controls">
	<label class="slider-control">
		number of cuts: {numCuts}
		<input type="range" bind:value={numCuts} min="1" max={maxCuts} step="1" />
	</label>

	<fieldset>
		<legend>cut type</legend>

		<div class="radio-group">
			<label>
				<input
					type="radio"
					name="cut-type"
					bind:group={cutType}
					value="vertical"
					checked
				/>
				vertical
			</label>

			<label>
				<input
					type="radio"
					name="cut-type"
					bind:group={cutType}
					value="radial"
				/>
				radial
			</label>
		</div>
	</fieldset>

	{#if cutType === 'radial'}
		<label class="slider-control">
			cut target height:
			{formatPercentageAsNegative(cutTargetDepthPercentage)} of outer radius
			<input
				type="range"
				bind:value={cutTargetDepthPercentage}
				min="0"
				max="1"
				step="0.01"
			/>
		</label>
	{/if}
</div>

<style>
	svg {
		margin-bottom: 2rem;
	}

	:global(line) {
		stroke: black;
	}

	:global(.cuts line) {
		stroke-dasharray: 5;
	}

	.controls {
		width: 300px;
	}

	.slider-control,
	fieldset {
		margin-bottom: 1rem;
	}

	.controls :is(label, input) {
		cursor: pointer;
	}

	.slider-control {
		display: inline-flex;
		flex-direction: column;
	}

	.radio-group {
		display: flex;
		flex-direction: column;
	}
</style>
