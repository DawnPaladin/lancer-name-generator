<script>
	export let name;
	export let tags;
</script>
<style>
	.demographic-tag {
		color: gray;
	}
</style>

<div>
	{ name.first } { name.last }{#if tags}, <span class="demographic-tag">{ name.gender } { name.origin }</span>{/if}
</div>