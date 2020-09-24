<script>
	import PopulationRow from './components/PopulationRow.svelte';
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

	regions.forEach(region => {
		region.enabled = true;
	});
	var totalPopulation = 0;
	
	function calcPopulation() {
		totalPopulation = 0;
		regions.forEach(region => {
			if (region.enabled) totalPopulation += region.population;
		});
		regions = regions;
	}

	function calcProportion() {
		regions.forEach(region => {
			region.proportion = region.enabled ? region.population / totalPopulation : 0;
		});
		regions = regions;
	}

	function update() {
		calcPopulation();
		calcProportion();
	}
	update();

	var howManyNames = 100;

	function generateNames() {
		names = [];
		for (let counter = 0; counter < howManyNames; counter++) {
			names.push(generateName());
		}
		names = names;
	}
	function generateName() {
		var region = selectRegion();
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
	function selectRegion() {
		var selector = Math.random() * totalPopulation; // select number between 0 and totalPopulation
		console.log("Initial selector", selector, regions);

		// subtract population of each region until the number goes negative
		for (let region of regions) {
			console.log(region.name, region.enabled, selector)
			if (region.enabled) selector -= region.population;
			if (selector < 0) return region;
		}
		return regions[regions.length - 1];
	}
	function draw(array) { // randomly select from, as in drawing from a deck of cards
		return array[Math.floor(Math.random() * array.length)];
	}
	
</script>
<style>
	th {
		text-align: left;
	}
</style>

<h1>Global Name Generator</h1>

<table>
	<thead>
		<tr>
			<th></th>
			<th>Region</th>
			<th>Population</th>
			<th>Proportion</th>
		</tr>
	</thead>
	{#each regions as region (region.name)}
		<PopulationRow region={region} on:regionUpdate={update} />
	{/each}
</table>

<p>Total population: {totalPopulation.toLocaleString()}</p>

<button on:click={generateNames}>Generate names</button>

<div class="names">
	{#each names as name}
		<NameRow name={name}/>
	{/each}
</div>