<script>
	export let height;
	export let numCuts;
	export let cutScale;
</script>

{#each { length: numCuts } as _, i}
	{@const x = cutScale(i)}

	<line x1={x} y1="0" x2={x} y2={height} />
{/each}
