<script>
	export let name;
	export let tags;
</script>

<div>
	{ name.first } { name.last }{#if tags}, { name.gender } { name.origin }{/if}
</div>