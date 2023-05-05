<script>
	import { scaleLinear } from 'd3-scale';
	import AxisX from './components/AxisX.svelte';
	import AxisY from './components/AxisY.svelte';
	import Onion from './components/Onion.svelte';
	import RadialCuts from './components/RadialCuts.svelte';
	import VerticalCuts from './components/VerticalCuts.svelte';

	let width = 600;
	let height = width / 2;
	let numLayers = 10;
	let maxCuts = numLayers;
	let numCuts = maxCuts;
	let cutType = 'vertical';

	const maxRadiusProportion = 0.8; // proportional to graph height
	const maxRadius = height * maxRadiusProportion;

	const rScale = scaleLinear().domain([0, numLayers]).range([0, maxRadius]);

	// TODO cut width should change with numCuts
	const cutScale = scaleLinear()
		.domain([0, numCuts])
		.range([width / 2, width / 2 - maxRadius]);
</script>

<svg {width} {height}>
	<AxisX {width} {height} />
	<AxisX {width} {height} isBottom />
	<!-- TODO responsive sizing: move y axis when screen resizes -->
	<AxisY {width} {height} />

	<Onion {width} {height} {numLayers} {rScale} />

	<g class="cuts">
		{#if cutType === 'vertical'}
			<VerticalCuts {height} {numCuts} {cutScale} />
		{:else if cutType === 'radial'}
			<RadialCuts />
		{/if}>
	</g>
</svg>

<div class="controls">
	<label class="cut-slider-control">
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

	.controls :is(label, input) {
		cursor: pointer;
	}

	.cut-slider-control {
		display: inline-flex;
		flex-direction: column;
	}

	.radio-group {
		display: flex;
		flex-direction: column;
	}
</style>
