<script>
	import { fade, fly } from 'svelte/transition';

	/** @type {{name: any, tags: any, index: any}} */
	let { name, tags, index } = $props();

	let origin = $derived(name.origins[0] === name.origins[1]
		? name.origins[0]
		: `${name.origins[0]}/${name.origins[1]}`);
</script>
<style>
	.demographic-tag {
		color: gray;
	}
</style>

<div in:fly|global={{ x: 150, duration: 500, delay: 250 + index * 12 }} out:fade|global={{ duration: 250 }}>
	{ name.first } { name.last }{#if tags}, <span class="demographic-tag">{ name.gender } { origin }</span>{/if}
</div>