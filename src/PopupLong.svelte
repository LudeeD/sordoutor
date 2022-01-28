<script>
    export let correcto;
    export let info;
    export let pontos;
    export let errados;

    import Costa from "./info/Costa.svelte";
    import Rio from "./info/Rio.svelte";
    import Catarina from "./info/Catarina.svelte";
    import Jeronimo from "./info/Jeronimo.svelte";
    import Xicao from "./info/Xicao.svelte";
    import Ines from "./info/Ines.svelte";
    import Ventura from "./info/Ventura.svelte";
    import Joao from "./info/Joao.svelte";
    import Rui from "./info/Rui.svelte";
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
{:else if info == "Jeronimo"}
    <Jeronimo />
{:else if info == "Xicao"}
    <Xicao />
{:else if info == "Ines"}
    <Ines />
{:else if info == "Ventura"}
    <Ventura />
{:else if info == "Joao"}
    <Joao />
{:else if info == "Rui"}
    <Rui />
{:else}
    <p>Algo correu mal :/</p>
{/if}

{#if correcto}
    <h3>✨ +1 Ponto ✨</h3>
    <img alt="gif vitorioso" src="./gifs/{pontos}.webp" />
{:else}
    <img alt="gif vitorioso" src="./gifs/{errados}_bad.webp" />
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
        img {
            max-height: 150px;
        }
    }
</style>
