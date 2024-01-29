<script>
	import { scaleLinear } from 'd3-scale';
	import { format } from 'd3-format';
	import 'carbon-components-svelte/css/white.css';
	import {
		RadioButtonGroup,
		RadioButton,
		Slider,
	} from 'carbon-components-svelte';
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

	// TODO move this math into external JS file
	let verticalCutThickness = radius;
	// TODO for a single vertical cut (i.e., cutting the onion half in half),
	//   quarterOnionArea = integral of circle height from 0 to r, dx (in first quadrant)
	//   https://ocw.mit.edu/courses/18-01sc-single-variable-calculus-fall-2010/28c569c5d8e79b7e1a2be1755de42d25_MIT18_01SCF10_Ses70a.pdf
	//   the expression used for quarterOnionArea here is an antiderivative F(x), and will need to have limits of integration applied as in F(b) - F(a)
	//     right now F(x) is F(verticalCutThickness) - F(0)
	$: quarterOnionArea =
		(radius * radius * Math.asin(verticalCutThickness / radius) +
			verticalCutThickness *
				Math.sqrt(
					radius * radius - verticalCutThickness * verticalCutThickness,
				)) /
		2;
	$: console.log({ quarterOnionArea });
	$: console.log('quarterOnionArea should be', (Math.PI * radius * radius) / 4);
</script>

<div class="container" style="--max-width: {width}px">
	<svg viewBox="{-width / 2} 0 {width} {height}">
		<AxisX {width} {height} />
		<AxisX {width} {height} isBottom />
		<!-- TODO responsive sizing: move y axis when screen resizes -->
		<AxisY {height} />

		<Onion {height} {numLayers} {rScale} />

		<Cuts {cutType} {numCuts} {height} {radius} {cutTargetDepthPercentage} />
	</svg>

	<div class="controls">
		<Slider
			labelText="number of cuts: {numCuts}"
			bind:value={numCuts}
			min={1}
			max={maxCuts}
			hideTextInput
		/>

		<RadioButtonGroup
			legendText="cut type"
			name="cut-type"
			bind:selected={cutType}
			orientation="vertical"
		>
			<RadioButton labelText="vertical" value="vertical" />
			<RadioButton labelText="radial" value="radial" />
		</RadioButtonGroup>

		<Slider
			labelText="cut target height:
		{formatPercentageAsNegative(cutTargetDepthPercentage)} of outer radius"
			bind:value={cutTargetDepthPercentage}
			min={0}
			max={1}
			maxLabel="100%"
			step={0.01}
			disabled={cutType !== 'radial'}
			hideTextInput
		/>
	</div>

	<p>WIP by <a href="https://aqandrew.com/">Andrew Aquino</a></p>
</div>

<style>
	.container {
		max-width: var(--max-width);
	}

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
		margin-inline: auto;
	}

	:global(.bx--form-item) {
		margin-bottom: 1rem;
	}

	p {
		margin-top: 4rem;
		text-align: center;
	}
</style>
