<script>
	import { onMount } from 'svelte';

	var protocol = import.meta.env.VITE_DATA_PROTOCOL;
	var ip = import.meta.env.VITE_DATA_IP;
	var port = import.meta.env.VITE_DATA_PORT;
	var url = protocol + "://" + ip + ":" + port + "/default/game"
	var rows = [];
	var tileTypes = {};
	var unitTypes = {};

	async function getGame() {
		const json = await fetch(url);
		var gameData = await json.json();
		console.log(gameData)
		rows = getRows(gameData);
		tileTypes = arrayToObjectById(gameData.ruleSet.tileTypes);
		unitTypes = arrayToObjectById(gameData.ruleSet.unitTypes);
		console.log(unitTypes);
		return gameData;
	}
	let data = getGame();


	function getRows(data){
		var map = data.map;
		var rows = [];
		//Convert data to a 2D array, based on coordiantes;
		for(var tile of map.tileTypeCoordinates){
			var coordinate = tile.coordinate;
			if(!rows[coordinate.y]) rows[coordinate.y] = [];
			rows[coordinate.y][coordinate.x] = tile.tileType;
		}
		return rows;
	}

	function arrayToObjectById(array){
		var object = {};
		array.forEach(value => object[value.id] = value);
		return object;
	}
</script>

{#await data}
	<p>Loading...</p>
{:then data}
	<table>
		{#each rows as row}
			<tr>
				{#each row as tileTypeId}
					<td>
						{tileTypes[tileTypeId].name}
					</td>
				{/each}
			</tr>
		{/each}
	</table>
{/await}