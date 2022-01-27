<script>
    export let correcto;
    export let info;

    import Costa from "./info/Costa.svelte";
    import Rio from "./info/Rio.svelte";
    import Catarina from "./info/Catarina.svelte";
</script>

{#if correcto}
    <h1>🎉 Está Correcto! 🍾</h1>
{:else}
    <h1>❌ Parece que não 😞</h1>
{/if}

{#if info == "Costa"}
    <Costa />
{:else if info == "Rio"}
    <Rio />
{:else if info == "Catarina"}
    <Catarina />
{:else}
    <p>2</p>
{/if}

{#if correcto}
    <h3>✨ +1 Ponto ✨</h3>
    <img alt="gif vitorioso" src="./gifs/1.webp" />
{/if}

<style>
    h1 {
        font-size: 2rem;
        text-align: center;
    }

    h3 {
        text-align: center;
    }

    img {
        display: block;
        margin-left: auto;
        margin-right: auto;
        max-width: 75%;
    }

    @media screen and (max-width: 600px) {
        h1 {
            font-size: 1.5rem;
        }
    }
</style>
