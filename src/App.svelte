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
	let cutType = 'vertical';
	let cutTargetDepthPercentage = 0;
	let debug = false;

	if (debug) {
		numLayers = 1;
		numCuts = 1;
	}

	const radiusPercentage = 0.8; // proportional to graph height
	const radius = height * radiusPercentage;

	const rScale = scaleLinear().domain([0, numLayers]).range([0, radius]);

	function formatPercentageAsNegative(n) {
		return format('.0%')(-n);
	}

	// TODO for a single vertical cut,
	//   area = integral of circle from 0 to radius
	$: area = numCuts;
	$: console.log({ area });
</script>

<svg {width} {height} viewBox="{-width / 2} 0 {width} {height}">
	<AxisX {width} {height} />
	<AxisX {width} {height} isBottom />
	<!-- TODO responsive sizing: move y axis when screen resizes -->
	<AxisY {height} />

	<Onion {height} {numLayers} {rScale} />

	<Cuts {cutType} {numCuts} {height} {radius} {cutTargetDepthPercentage} />
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

	<label class="slider-control">
		cut target height:
		{formatPercentageAsNegative(cutTargetDepthPercentage)} of outer radius
		<input
			type="range"
			bind:value={cutTargetDepthPercentage}
			min="0"
			max="1"
			step="0.01"
			disabled={cutType !== 'radial'}
		/>
	</label>
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

	.slider-control:has([disabled]),
	.slider-control input[disabled] {
		cursor: not-allowed;
	}

	.radio-group {
		display: flex;
		flex-direction: column;
	}
</style>
