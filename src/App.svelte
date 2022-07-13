<script>  

  //import {onMount} from 'svelte'
  //import { debug } from 'svelte/internal'
  import {fly} from 'svelte/transition'
  import MovieItem from './Movie/Item.svelte'

  const APIKEY = '29ed1d64cc3508c30f08131eb1860d99'
  const BASEURL = 'https://api.themoviedb.org/3'
  const APISETTINGS = `?api_key=${APIKEY}&language=es-MX`

  /* let movies = []

  function fetchMovies (){
    const URL = `${BASEURL}/discover/movie${APISETTINGS}&sort_by=popularity.desc`

    fetch(URL).then(res => res.json()).then(({results}) =>{movies =  results,  console.log(movies)})

  } */

  const movies =  (async () => {

    const URL = `${BASEURL}/discover/movie${APISETTINGS}&sort_by=popularity.desc`
    const response = await fetch(URL)

    return await response.json()
  })()

  let likedMovies = []

  function toggleLike(event){
    //console.log(event)

    const movie = event.detail

    let index = likedMovies.findIndex(m=> m.id === movie.id)

    if(index >= 0){
      likedMovies.splice(index,1)
      console.log(likedMovies)
    }else{
      likedMovies.push(movie)
      console.log(likedMovies)
    }

    likedMovies = likedMovies

  }

  $: like =  (id) => {
    let index = likedMovies.findIndex(m => m.id === id)

    return index >= 0
  }

  /* onMount(()=>{
    console.log('Component has been mounted!')
    fetchMovies()
  }) */

</script>

<svelte:head>
  <title>App Movies with Svelte</title>
  <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.2.0-beta1/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-0evHe/X+R7YkIZDRvuzKMRqM+OrBnVFBL6DOitfPri4tjfHxaWutUpFmBp4vmVor" crossorigin="anonymous">
</svelte:head>

<main class="container-fluid">
  <div class="row">
    <div class="col-12 col-md-6 col-lg-8 border panel">
      <h2>Popularity Movies</h2>

      <div class="row">
        {#await movies}
        <!-- promise is pending-->
          <div class="col-12"><p>loading...</p></div>
        {:then data}
          <!-- {@debug data} -->
          <!-- promise was fulfilled-->
          {#each data.results as movie}
            <div class="col-12 col-md-6 col-lg-4 p-1">
              <MovieItem like={like(movie.id)} id={movie.id} title={movie.title} overview={movie.overview} cover={movie.poster_path} on:onToggleLike={toggleLike}></MovieItem>
            </div>
          {/each}
        {/await}
      </div>
      
    </div>
    <div class="col-12 col-md-6 col-lg-4 border panel">
      <h2>Favorite Movies</h2>
      <div class="row">
        {#if likedMovies.length}
          {#each likedMovies as movie, i (movie.id)}
            <div in:fly="{{duration:200, y:20}}"  out:fly="{{duration:800, y:-20}}" class="col-12 col-md-6 col-lg-4 p-1">
              <MovieItem like={like(movie.id)} id={movie.id} title={movie.title} overview="" cover={movie.cover} on:onToggleLike={toggleLike}></MovieItem>
            </div>
          {/each}
        {:else}
            <div class="col-12"><p>No favorite movies</p></div>
        {/if}
        
      </div>
    </div>
  </div>
</main>

<style>
  :root {
    font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen,
      Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;
  }

  .panel {
    height: 100vh;
    overflow: auto;
  }

  main {
    text-align: center;
    padding: 1em;
    margin: 0 auto;
  }

  img {
    height: 16rem;
    width: 16rem;
  }

  h1 {
    color: #ff3e00;
    text-transform: uppercase;
    font-size: 4rem;
    font-weight: 100;
    line-height: 1.1;
    margin: 2rem auto;
    max-width: 14rem;
  }

  p {
    max-width: 14rem;
    margin: 1rem auto;
    line-height: 1.35;
  }

  @media (min-width: 480px) {
    h1 {
      max-width: none;
    }

    p {
      max-width: none;
    }
  }
</style>
