<script>
	import { createEventDispatcher } from 'svelte';
	const dispatch = createEventDispatcher();
	/** @type {{region: any}} */
	let { region = $bindable() } = $props();
	
	function changeCheckbox() {
		region.enabled = !region.enabled;
		dispatch('regionUpdate', {
			name: region.name,
			enabled: region.enabled
		});
	}
</script>

<label>
	<input type="checkbox" bind:checked={region.enabled} oninput={changeCheckbox} />
	{region.name}
</label>
