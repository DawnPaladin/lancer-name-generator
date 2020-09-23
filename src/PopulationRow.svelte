<script>
	import { createEventDispatcher } from 'svelte';
	const dispatch = createEventDispatcher();
	export let region;
	
	function changeCheckbox() {
		region.enabled = !region.enabled;
		console.log({
			name: region.name,
			enabled: region.enabled
		});
		dispatch('regionUpdate', {
			name: region.name,
			enabled: region.enabled
		});
	}
</script>
<style>
	.align-right {
		text-align: right;
	}
</style>

<tr>
	<td>
		<input type="checkbox" bind:checked={region.enabled} on:input={changeCheckbox} />
	</td>
	<td>
		{region.name}
	</td>
	<td class="align-right">
		{region.population.toLocaleString()}
	</td>
</tr>