<script>
	export let width;
	export let height;
	export let isBottom = false;

	let y = isBottom ? height : 0;
</script>

<line x1="0" y1={y} x2={width} y2={y} stroke="black" />
