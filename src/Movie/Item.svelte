<script>

    import {createEventDispatcher} from 'svelte'

    export let id
    export let title
    export let overview
    export let cover
    export let like

    $: coverURL = 'https://image.tmdb.org/t/p/w185_and_h278_bestv2'+cover

    const dispatch = createEventDispatcher()

    function toggleLike(){
        dispatch('onToggleLike', {id, title, overview, cover})
    }

</script>

<div class="card">
    <img src={coverURL} class="card-img-top" alt="..." />
    <div class="card-body">
        <h5 class="card-title">{title}</h5>
        <p class="card-text">{overview}</p>
    </div>
    <div class="text-center mb-2">
        <button class={like ? 'btn btn-secondary' : 'btn btn-primary'} on:click={toggleLike}>{like ? 'Unlike': 'Like'}</button>
    </div>
</div>
