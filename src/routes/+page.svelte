<script>
    // src/main.js
    import "../app.css";
    import { onMount } from "svelte";
    import { writable } from 'svelte/store';

    
    let searchinput = ''; // Initialize as an empty string
    let fetchedData = writable([]); // Use a writable store for fetched data
    let api_key = import.meta.env.OMDB_KEY

    onMount(async () => {
    let response = await fetch(`http://www.omdbapi.com/?i=tt3896198&apikey=29de6b07&s=batman`);
    let json = await response.json();
    fetchedData.set(json.Search);
})

let searchmovie = async (inputvalue) => {
        let response = await fetch(`http://www.omdbapi.com/?i=tt3896198&apikey=29de6b07&s=${inputvalue}`);
        let json = await response.json();
        fetchedData.set(json.Search);
}


</script>    
<main class="bg-gray-700">  
    <nav class="bg-teal-600 p-4">
        <div class="flex items-center justify-between">
            <div class="underline decoration-double decoration-black-500 text-xxl">FaizanMovies</div>
            <ul class="flex space-x-4">
                <li><a href="#" class="text-white hover:underline">Home</a></li>
                <li><a href="#" class="text-white hover:underline">About</a></li>
                <li><a href="#" class="text-white hover:underline">Contact</a></li>
            </ul>
        </div>
    </nav>
    <div class="flex items-center space-x-2 bg-gray-100 p-3 rounded-md shadow-md">
        <div class="relative flex-grow">
            <input type="search" placeholder="Search..." class="w-full py-2 px-4 rounded-md focus:outline-none focus:ring focus:ring-black-500" bind:value={searchinput}>
            <div class="absolute inset-y-0 left-0 flex items-center pl-3">
            </div>
        </div>
        <button class="bg-blue-500 hover:bg-blue-600 text-white px-4 py-2 rounded-md" on:click={() => searchmovie(searchinput)}>Search</button>
    </div>
    
    {#if $fetchedData}
    <div class="pt-24">
        <div class="grid grid-cols-2 md:grid-cols-3 lg:grid-cols-3 gap-6">
            {#each $fetchedData as result}
                <div class="bg-gray-800 text-gray-100 p-4 rounded-lg shadow-2xl">
                    <img class="w-full mb-4" src={result.Poster} alt="Movie"/>
                    <h2 class="text-2xl font-semibold mb-2">{result.Title}</h2>
                    <p class="text-sm mb-2">Type: {result.Type}</p>
                    <p class="text-sm mb-2">Year: {result.Year}</p>
                </div>
            {/each}
        </div>
    </div>
    {:else}
    <p>Sorry, No results Found</p>
    {/if}
        
    <footer class="footer p-10 bg-base-200 text-base-content bg-teal-800 text-white p-4 text-center">
        <div><h1>Thank You</h1></div>
    </footer>
</main>
