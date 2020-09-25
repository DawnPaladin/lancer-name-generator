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

	var howManyNames = 20;

	function generateNames() {
		names = [];
		for (let counter = 0; counter < howManyNames; counter++) {
			names.push(generateName());
		}
		names = names;
	}
	function generateName() {
		var region = draw(regions);
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
	th {
		/* text-align: left; */
	}
	.flex-row {
		display: flex;
		flex-direction: row;
	}
</style>

<h1>Lancer Name Generator</h1>

<div class="flex-row">
	<div>
		<table>
			<thead>
				<tr>
					<th></th>
					<th>Region</th>
				</tr>
			</thead>
			{#each regions as region (region.name)}
				<RegionRow region={region} on:regionUpdate={update} />
			{/each}
		</table>
		
		<button on:click={generateNames}>Generate names</button>
	</div>
	
	<div class="names">
		{#each names as name}
			<NameRow name={name}/>
		{/each}
	</div>
</div>