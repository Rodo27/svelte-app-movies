<script>  

  import {onMount} from 'svelte'
  import MovieItem from './Movie/Item.svelte'

  const APIKEY = '29ed1d64cc3508c30f08131eb1860d99'
  const BASEURL = 'https://api.themoviedb.org/3'
  const APISETTINGS = `?api_key=${APIKEY}&language=es-MX`

  let movies = []

  function fetchMovies (){
    const URL = `${BASEURL}/discover/movie${APISETTINGS}&sort_by=popularity.desc`

    fetch(URL).then(res => res.json()).then(({results}) =>{movies =  results})

  }

  onMount(()=>{
    console.log('Component has been mounted!')
    fetchMovies()
  })

</script>

<svelte:head>
  <title>App Movies with Svelte</title>
  <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.2.0-beta1/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-0evHe/X+R7YkIZDRvuzKMRqM+OrBnVFBL6DOitfPri4tjfHxaWutUpFmBp4vmVor" crossorigin="anonymous">
</svelte:head>

<main class="container">
  <div class="row">
    {#each movies as movie}
      <div class="col-12 col-md-6 col-lg-3 p-1">
        <MovieItem></MovieItem>
      </div>
    {/each}
  </div>
</main>

<style>
  :root {
    font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen,
      Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;
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
