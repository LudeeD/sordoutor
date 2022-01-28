<script>
    import { onMount } from "svelte";
    import { getContext } from "svelte";
    import { bubble } from "svelte/internal";
    import { fly } from "svelte/transition";
    import PopupLong from "./PopupLong.svelte";

    const { open } = getContext("simple-modal");

    let pontos = 0;
    let errados = 0;
    let estado = "inicial";

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
        {
            id: "Jeronimo",
            nome: "Jerónimo de Sousa",
            img: "./images/Jeronimo.jpg",
            doutor: false,
            sexo: false,
        },
        {
            id: "Xicao",
            nome: "Francisco Rodrigues dos Santos",
            img: "./images/Xicao.jpg",
            doutor: false,
            sexo: false,
        },
        {
            id: "Ines",
            nome: "Inês Sousa Real",
            img: "./images/Ines.jpg",
            doutor: false,
            sexo: true,
        },
        {
            id: "Ventura",
            nome: "André Ventura",
            img: "./images/Ventura.png",
            doutor: true,
            sexo: false,
        },
        {
            id: "Joao",
            nome: "João Cotrim de Figueiredo",
            img: "./images/Joao.png",
            doutor: false,
            sexo: false,
        },
        {
            id: "Rui",
            nome: "Rui Tavares",
            img: "./images/Rui.png",
            doutor: true,
            sexo: false,
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

        if (ordem_a_mostar == concorrentes.length) {
            estado = "final";
        }
        mostrar = concorrentes[ordem[ordem_a_mostar]];
    }

    const doutor = () => {
        let acertou = mostrar["doutor"];

        if (acertou) {
            pontos = pontos + 1;
        } else {
            errados = errados + 1;
        }

        mostrarInfo(acertou);
    };

    const plebe = () => {
        let acertou = !mostrar["doutor"];

        if (acertou) {
            pontos = pontos + 1;
        } else {
            errados = errados + 1;
        }

        mostrarInfo(acertou);
    };

    const mostrarInfo = (acertou) => {
        open(
            PopupLong,
            {
                correcto: acertou,
                info: mostrar["id"],
                pontos: pontos,
                errados: errados,
            },
            { transitionWindow: fly },
            {
                onClose: () => {
                    next();
                },
            }
        );
    };

    const começarJogo = () => {
        estado = "jogar";
    };

    function updateclipboard() {
        let newClip = "Sôr doutor?\nAcertei " + pontos + ".💪🧐";
        navigator.clipboard.writeText(newClip).then(
            function () {
                alert(
                    "Copiei os resultados para o clipboard. Dá paste onde quiseres!"
                );
            },
            function () {
                alert("Algo não correu bem");
            }
        );
    }
</script>

<div class="center">
    {#if estado == "jogar"}
        {#if mostrar}
            {#if mostrar["sexo"]}
                <h1>A {mostrar["nome"]} é Sôr Doutora?</h1>
            {:else}
                <h1>O {mostrar["nome"]} é Sôr Doutor?</h1>
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
    {:else if estado == "inicial"}
        <h1>Sôr Doutor?</h1>
        <p>
            Durante a campanha das <a
                href="https://pt.wikipedia.org/wiki/Elei%C3%A7%C3%B5es_legislativas_portuguesas_de_2022"
                >legislativas 2022</a
            > toda a gente falou da TAP ✈️, do rendimento mínimo universal 💰, da
            pena de morte 💀, do orçamento chumbado 📉, das pontes desfeitas 💣 e
            refeitas 🔨.
        </p>
        <p>
            Mas ninguém levantou o problema que vale a pena discutir 📝...
            Sempre que falam uns com os outros e mesmo quando são entrevistados
            é Sôr doutor 🤓 para aqui e para ali
        </p>

        <h3>Mas são todos Sôr Doutor?</h3>

        <img
            style="max-width: 200px"
            alt="pessoa a mergulhar"
            src="./gifs/investigar.webp"
        />

        <button style="margin-top: 5px;" on:click={começarJogo}
            >Clica para investigar</button
        >
    {:else if estado == "final"}
        <h1 style="margin-top: 5vh;">✨ Fim! ✨</h1>

        <h2>Pontuação Final: {pontos}/{pontos_máximos}</h2>

        <h4>
            Partilha com os teus amigos para ver quem é que tem futuro como
            comentador político, ou não
        </h4>

        <img
            style="max-width: 300px"
            alt="gif de senhora a tentar cantar"
            src="./gifs/fim.webp"
        />
        <button style="margin-top: 1vh" on:click={updateclipboard}
            >Anunciar a boa nova</button
        >

        <p style="margin-top: 10vh;">
            Então e quem trouxe quem trouxe esta pérola?
        </p>
        <p>
            Não foi o <a href="https://luissilva.eu">Pingo Doce</a>
        </p>
    {/if}
</div>

<style>
    @media screen and (max-width: 600px) {
        h1 {
            font-size: 1.5rem;
        }
    }
    h2 {
        margin-top: 5px;
    }

    h4 {
        text-align: justify;
    }

    img {
        max-height: 400px;
        width: auto;
    }

    p {
        max-width: 400px;
        text-align: justify;
        margin-top: 5px;
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
