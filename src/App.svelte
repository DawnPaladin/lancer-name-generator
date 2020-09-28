<script>
	import RegionRow from './components/RegionRow.svelte';
	import NameRow from './components/NameRow.svelte';

	import Bangladesh from './regions/Bangladesh.json';
	import Brazil from './regions/Brazil.json';
	import China from './regions/China.json';
	import India from './regions/India.json';
	import Indonesia from './regions/Indonesia.json';
	import Japan from './regions/Japan.json';
	import Mexico from './regions/Mexico.json';
	import Nigeria from './regions/Nigeria.json';
	import Pakistan from './regions/Pakistan.json';
	import Russia from './regions/Russia.json';
	import USA from './regions/USA.json';
	var regions = [ Bangladesh, Brazil, China, India, Indonesia, Japan, Mexico, Nigeria, Pakistan, Russia, USA ];
	var names = [];
	var tags = true;
	var sampleSize = 15;
	var allRegionsEnabled;
	$: allRegionsEnabled = enabledRegions().length == regions.length;

	regions.forEach(region => {
		region.enabled = true;
	});
	var multiplierTotal = 0;
	
	function calcProportion() {
		regions.forEach(region => {
			region.proportion = region.enabled ? region.multiplier / multiplierTotal : 0;
		});
		regions = regions;
	}

	function update() {
		calcProportion();
	}
	update();

	function generateNames() {
		names = [];
		for (let counter = 0; counter < sampleSize; counter++) {
			names.push(generateName());
		}
		names = names;
	}
	generateNames();
	function generateName() {
		var region = draw(enabledRegions());
		var gender = Math.random() > 0.5 ? "male" : "female";
		var firstNames = gender == "male" ? region["male-first"] : region["female-first"];
		var lastNames = region.last;
		return {
			first: draw(firstNames),
			last: draw(lastNames),
			gender,
			origin: region.adjective
		}
	}
	function draw(array) { // randomly select from, as in drawing from a deck of cards
		return array[Math.floor(Math.random() * array.length)];
	}

	function enabledRegions() {
		return regions.filter(region => region.enabled);
	}
	function toggleAllRegions() {
		if (allRegionsEnabled) {
			regions.forEach(region => { // disable all regions
				region.enabled = false;
			});
		} else {
			regions.forEach(region => { // enable all regions
				region.enabled = true;
			});
		}
		regions = regions;
	}
	
</script>
<style>
	:global(body) {
		background: url(/img/hex-grid.png);
		background-size: 86px 149px;
	}
	@media (prefers-reduced-motion: no-preference) {
		:global(body) {
			animation: drift 20s infinite linear;
		}
	}
	@keyframes drift {
		from {
			background-position: 0px 0px;
		} 
		to {
			background-position: -172px -149px;
		}
	}
	main {
		margin: auto;
		width: 525px;
	}
	h1 {
		text-align: center;
	}
	.flex-row {
		display: flex;
		flex-direction: row;
	}
	.right-column {
		min-width: 20em;
		margin: 0 0 0 2em;
	}
	.column-header {
		border-bottom: 1px solid gray;
		margin-bottom: 3px;
	}
	.error {
		font-family: "Courier New", monospace;
		font-size: 12px;
	}
	.names {
		overflow: hidden;
	}
</style>
<svelte:head>
	<title>Union Population Sampling Tool</title>
</svelte:head>

<main>

	<h1>Union Population Sampling Tool</h1>
	
	<div class="flex-row">
		<div class="left-column">
			<div class="column-header">
				<input type="checkbox" checked={allRegionsEnabled} on:click={toggleAllRegions} />
				<strong>Ancestral origin</strong>
			</div>
			{#each regions as region (region.name)}
				<RegionRow region={region} on:regionUpdate={update} />
			{/each}
			<label>
				Sample size:
				<input type="number" bind:value={sampleSize} style="width: 4em"/>
			</label>
			<button on:click={generateNames}>Generate sample</button>
			<div class="error">
				<span style="color: crimson">//ATTEND//</span> Missing origins detected. Please <a href="https://github.com/DawnPaladin/lancer-name-generator#contributing">install additional origins</a>.
			</div>
		</div>
		
		<div class="right-column">
			<div class="flex-row column-header" style="justify-content: space-between">
				<strong>Output</strong>
				<label>
					<input type="checkbox" bind:checked={tags} /> Demographic tags
				</label>
			</div>
			<div class="names" style="height: {sampleSize * 1.5}em">
				{#each names as name, index (name)}
					<NameRow name={name} tags={tags} index={index} />
				{/each}
			</div>
		</div>
	</div>
</main>