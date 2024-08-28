 <!-- A simple project showing how to use axios within Svelte -->

<!-- DEPENDENCIES -->
<!-- npm install -->
<!-- npx degit sveltejs/template [project-name] -->

 <!-- https://www.tvmaze.com/ -->
 <!--  API: http://api.tvmaze.com/search/shows?q=girls -->

 <!-- CODE -->
<script>
	let tvShow; //Keeps track of the search bar input
	
	//Will handle the submission after button is clicked.
	//-----
	const handleSubmit = async (event) =>{
		const config = {params: {q: tvShow}} //axios headers
		const res = await axios.get(`http://api.tvmaze.com/search/shows`, config); //axois call
		const { data } = res; //data within the returned API.

		//Remove all children within 'movies'
		const movies = document.querySelector(".movies")
		while (movies.firstChild) {
			movies.removeChild(movies.firstChild);
		}

		//Loop through data:
		for (let result of data){
			makeImg(result); //create a new img from API data.
		}
	}

	//Will create an image for any corresponding tv show
	//-----
	const makeImg = (result) =>{
		const movies = document.querySelector(".movies")
		const { show } = result


		const newImg = document.createElement("img");
		newImg.src = show.image.medium
		movies.append(newImg)
	}
</script>

<!-- HTML -->
<main id="root">
	<!-- LINK THAT AXIOS NEEDS IN ORDER TO WORK -->
	<script src="https://cdn.jsdelivr.net/npm/axios/dist/axios.min.js"></script>

	<!-- Content -->
	<h1>Television Show Search</h1>
	<form on:submit|preventDefault={handleSubmit} id="searchForm">
		<input type="text" placeholder="Show Title" nam="query" bind:value={tvShow}>
		<button>Search</button>
	</form>
	<!-- List -->
	<div class="movies"></div>
</main>

<!-- CSS -->
<style>
	#root {
		text-align: center;
	}
</style>