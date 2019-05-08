<template lang="html">
	<form id="sightings-form" v-on:submit="addSighting" method="post">
		<h2>Add a Sighting</h2>
		<div class="formWrap">
			<label for="species">Species:</label>
			<input type="text" id="species" v-model:species="species" />
		</div>
		<div class="formWrap">
			<label for="location">Location:</label>
			<input type="text" id="location" v-model:location="location" />
		</div>
		<div class="formWrap">
			<label for="date">Date:</label>
			<input type="date" id="date" v-model:date="date"/>
		</div>

		<input type="submit" value="Save" id="save"/>
	</form>
</template>

<script>
import { eventBus } from '@/main.js'
export default {
	name: "SightingsForm",
	data(){
		return {
			species: null,
			location: null,
			date: null
		}
	},
	methods: {
		addSighting(event) {
			event.preventDefault()
			const url = 'http://localhost:3000/api/sightings'
			const sighting = {
				species: this.species,
				location: this.location,
				date: this.date
			}

			fetch(url, {
				method: 'POST',
				body: JSON.stringify(sighting),
				headers: { 'Content-Type': 'application/json' }
			})
			// .then(res => res.json())
			.then(res => {
				eventBus.$emit("refresh-data", res)
			})
		}



	}
}
</script>

<style lang="css" scoped>
h2 {
	margin: 10px 0;
	padding: 0;
}

form {
	width: 75%;
	margin: 0 auto;
	background: rgba(255, 255, 255, 0.7);
	padding: 20px;
	margin-bottom: 40px;
}

label {
	min-width: 100px;
	display: inline-block;
}

.formWrap {
	margin-bottom: 10px;
}
</style>
