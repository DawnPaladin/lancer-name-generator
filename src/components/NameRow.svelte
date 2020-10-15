<script>
	import { fade, fly } from 'svelte/transition';

	export let name;
	export let tags;
	export let index;

	if (name.origins[0] == name.origins[1]) {
		var origin = name.origins[0];
	} else {
		var origin = name.origins[0]+'/'+name.origins[1]
	}
</script>
<style>
	.demographic-tag {
		color: gray;
	}
</style>

<div in:fly={{ x: 150, duration: 500, delay: 250 + index * 12 }} out:fade={{ duration: 250 }}>
	{ name.first } { name.last }{#if tags}, <span class="demographic-tag">{ name.gender } { origin }</span>{/if}
</div>