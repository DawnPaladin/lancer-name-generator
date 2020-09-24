<script>
	import PopulationRow from './PopulationRow.svelte';

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