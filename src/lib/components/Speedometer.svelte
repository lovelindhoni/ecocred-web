<script lang="ts">
	import Speedometer from 'svelte-speedometer';
    export let value;
</script>

<Speedometer
	minValue={0}
	maxValue={10}
	value={value}
	needleColor="red"
	startColor="red"
	segments={10}
	endColor="green"
/>
