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
	let numCuts = numLayers + 1;
	let cutType = 'vertical';

	const maxRadiusProportion = 0.8; // proportional to graph height
	const maxRadius = height * maxRadiusProportion;

	const rScale = scaleLinear().domain([0, numLayers]).range([0, maxRadius]);

	const cutScale = scaleLinear()
		.domain([0, numLayers])
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
			<VerticalCuts {width} {height} {numCuts} {cutScale} />
		{:else if cutType === 'radial'}
			<RadialCuts />
		{/if}>
	</g>
</svg>

<div class="controls">
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
	:global(line) {
		stroke: black;
	}

	:global(.cuts line) {
		stroke-dasharray: 5;
	}

	.controls label {
		cursor: pointer;
	}

	.radio-group {
		display: flex;
		flex-direction: column;
	}
</style>
