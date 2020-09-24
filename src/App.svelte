<script>
	import PopulationRow from './PopulationRow.svelte';
	
	let regions = [
		{ name: "China", population: 1433783686 },
		{ name: "India", population: 1366417754 },
		{ name: "United States", population: 329064917 },
		{ name: "Indonesia", population: 270625568 }
	]
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

<p>
	Total population: {totalPopulation.toLocaleString()}
</p>