<script>
    import { onMount } from "svelte";
    import { getContext } from "svelte";
    import { fly } from "svelte/transition";
    import PopupLong from "./PopupLong.svelte";

    const { open } = getContext("simple-modal");

    let pontos = 0;

    function shuffle(array) {
        let currentIndex = array.length,
            randomIndex;

        // While there remain elements to shuffle...
        while (currentIndex != 0) {
            // Pick a remaining element...
            randomIndex = Math.floor(Math.random() * currentIndex);
            currentIndex--;

            // And swap it with the current element.
            [array[currentIndex], array[randomIndex]] = [
                array[randomIndex],
                array[currentIndex],
            ];
        }

        return array;
    }

    let concorrentes = [
        {
            id: "Costa",
            nome: "António Costa",
            img: "./images/Costa.jpg",
            doutor: false,
            sexo: false,
        },
        {
            id: "Rio",
            nome: "Rui Rio",
            img: "./images/Rio.jpg",
            doutor: false,
            sexo: false,
        },
        {
            id: "Catarina",
            nome: "Catarina Martins",
            img: "./images/Catarina.png",
            doutor: false,
            sexo: true,
        },
    ];

    let pontos_máximos = concorrentes.length;
    let ordem = Array.from(Array(concorrentes.length).keys());
    let ordem_a_mostar = 0;

    let mostrar;
    onMount(async () => {
        shuffle(ordem);

        mostrar = concorrentes[ordem[ordem_a_mostar]];

        console.log(mostrar);
    });

    function next(event) {
        ordem_a_mostar = ordem_a_mostar + 1;

        mostrar = concorrentes[ordem[ordem_a_mostar]];
    }

    const doutor = () => {
        let acertou = mostrar["doutor"];

        if (acertou) {
            pontos = pontos + 1;
        }

        mostrarInfo(acertou);
    };

    const plebe = () => {
        let acertou = !mostrar["doutor"];

        if (acertou) {
            pontos = pontos + 1;
        }

        mostrarInfo(acertou);
    };

    const mostrarInfo = (acertou) => {
        open(
            PopupLong,
            { correcto: acertou, info: mostrar["id"] },
            { transitionWindow: fly },
            {
                onClose: () => {
                    next();
                },
            }
        );
    };
</script>

<div class="center">
    {#if mostrar}
        {#if mostrar["sexo"]}
            <h1>A {mostrar["nome"]} é Sõr Doutora?</h1>
        {:else}
            <h1>O {mostrar["nome"]} é Sõr Doutor?</h1>
        {/if}
        <div>
            <img alt={mostrar["nome"]} src={mostrar["img"]} />
        </div>
    {:else}
        <h1>Loading...</h1>
    {/if}

    <div class="buttons">
        <div class="action_btn">
            <button
                name="submit"
                class="action_btn"
                type="submit"
                on:click={doutor}>🤓 Doutor</button
            >
            <button
                name="submit"
                class="action_btn cancel"
                type="submit"
                on:click={plebe}
                >🤠 Plebe
            </button>
            <p id="saved" />
        </div>
    </div>

    <h2>Pontuação: {pontos} / {pontos_máximos}</h2>
</div>

<style>
    h2 {
        margin-top: 5px;
    }
    img {
        max-height: 400px;
        width: auto;
    }

    .center {
        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: center;
        max-width: 800px;
        margin: auto;
    }

    .buttons {
        margin-top: 5px;
    }

    .action_btn {
        width: 150px;
        margin: 0 auto;
        display: inline;
    }
</style>
