<script>
	import RegionRow from './components/RegionRow.svelte';
	import NameRow from './components/NameRow.svelte';

	import Africa from './regions/Africa.json';
	import Bangladesh from './regions/Bangladesh.json';
	import Brazil from './regions/Brazil.json';
	import China from './regions/China.json';
	import India from './regions/India.json';
	import Indonesia from './regions/Indonesia.json';
	import Pakistan from './regions/Pakistan.json';
	import Russia from './regions/Russia.json';
	import USA from './regions/USA.json';
	var regions = [ Africa, Bangladesh, Brazil, China, India, Indonesia, Pakistan, Russia, USA ];
	var names = [];
	var tags = true;

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

	var sampleSize = 20;

	function generateNames() {
		names = [];
		for (let counter = 0; counter < sampleSize; counter++) {
			names.push(generateName());
		}
		names = names;
	}
	generateNames();
	function generateName() {
		var enabledRegions = regions.filter(region => region.enabled);
		var region = draw(enabledRegions);
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
	
</script>
<style>
	main {
		margin: auto;
		max-width: 525px;
	}
	.flex-row {
		display: flex;
		flex-direction: row;
	}
	.right-column {
		min-width: 20em;
		margin: 0 0 0 2em;
	}
	.error {
		font-family: "Courier New", monospace;
	}
</style>

<main>

	<h1>Union Population Sampling Tool</h1>
	
	<div class="flex-row">
		<div class="left-column">
			<input type="checkbox" style="visibility: hidden"/> <!-- for spacing -->
			<strong>Ancestral origin</strong>
			{#each regions as region (region.name)}
				<RegionRow region={region} on:regionUpdate={update} />
			{/each}
			<label>
				Sample size:
				<input type="number" bind:value={sampleSize} style="width: 4em"/>
			</label>
			<button on:click={generateNames}>Generate sample</button>
			<div class="error">
				[<span style="color: crimson">Error:</span> Missing origins detected. Please install additional origins.]
			</div>
		</div>
		
		<div class="right-column">
			<div class="flex-row" style="justify-content: space-between">
				<strong>Output</strong>
				<label>
					<input type="checkbox" bind:checked={tags} /> Demographic tags
				</label>
			</div>
			<div class="names">
				{#each names as name}
					<NameRow name={name} tags={tags}/>
				{/each}
			</div>
		</div>
	</div>
</main>