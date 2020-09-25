# Lancer name generator

This is a gamemaster's tool for [Lancer, the Mech RPG](https://massif-press.itch.io/corebook-pdf-free), which takes place far in the future, with humanity spread far into the Milky Way. It generates names from a variety of Earth cultures.

The app is built using [Svelte](https://svelte.dev). To run it locally, you'll need [NodeJS](https://nodejs.org/en/) installed; clone the repository, run `npm install`, and then run `npm run dev`.

## Contributing

Presently the tool knows names from the top ten countries by population plus the continent of Africa. Much of humanity is still unrepresented. I'd like to fix this, and I need your help!

To add names from a region of the world, you need to create a JSON file. ([This site](https://jsoneditoronline.org/) is useful for editing JSON files.) Here's how the file should be laid out:

```json
{
	"name": "Sampletopia",
	"adjective": "Samplese",
	"male-first": [
		"John",
		"Joe",
		"Bob"
	],
	"female-first": [
		"Jane",
		"Mary",
		"Emily"
	],
	"last": [
		"Smith",
		"Johnson",
		"Williams"
	]
}
```

- **name:** Name of the country/region. This will be shown in the list on the left.
- **adjective:** Adjective form of the name, used in the demographic tags. If the name is "Africa", the adjective is "African".
- **male-first:** An array of masculine first names. You can put as many of these as you like, but each country should have at least 100.
- **female-first:** Same as above, but feminine.
- **last:** An array of last names.

If you want to open a pull request, the file goes in the /src/regions folder, and you'll need to `import` it at the top of /src/App.svelte and add it to the `regions` array. Or you can just send it to me on Discord - join [Pilot NET](https://discord.gg/rgkbcCt) and send it to DawnPaladin#5461.