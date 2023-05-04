<script>
	import AxisX from './components/AxisX.svelte';
	import AxisY from './components/AxisY.svelte';

	let width = 600;
	let height = width / 2;
</script>

<svg {width} {height}>
	<AxisX {width} {height} />
	<AxisX {width} {height} isBottom />
	<!-- TODO responsive sizing: move y axis when screen resizes -->
	<AxisY {width} {height} />
</svg>
