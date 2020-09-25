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

<label>
	<input type="checkbox" bind:checked={region.enabled} on:input={changeCheckbox} />
	{region.name}
</label>
