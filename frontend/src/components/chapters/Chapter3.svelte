<script lang="ts">
    import Navigation from "../Navigation.svelte";
    import CubeNamed from "../CubeNamed.svelte";
    import List from "../List.svelte";
    import Terminal from "../Terminal.svelte";

    export let switchers;

    let tabs = [
        { id: 1, active: false },
        { id: 0, active: true },
    ];

    const cubes = {
        image: "image",
        container: "container",
    };

    const hubItems = [
        {
            no: 1,
            title: "docker search <nazwa>",
            content:
                "Wyszukuje wszystkie obrazy o nazwie podobnej do <nazwa> w repozytorium Docker Hub.",
        },
        {
            no: 2,
            title: "docker pull obraz:tag",
            content: "Pobiera na dysk wybrany obraz z Docker Huba.",
        },
    ];

    let command;
    let output = {
        "docker search python": `<table><tr><th>NAME</th><th>DESCRIPTION</th><th>OFFICIAL</th></tr>
            <tr><td>python</td><td>Python is an interpeted language ...</td><td>[OK]</td></tr>
            <tr><td>kaggle/python</td><td>Docker image for Python scripts run on Kaggle</td><td></td></tr></table>`,
        "docker pull python": `Status: Downloaded newer image for python:latest`,
        "docker pull python:slim": `Status: Downloaded newer image for python:slim`,
        "docker pull kaggle/python": `Status: Downloaded newer image for kaggle/python:latest`,
        "docker image ls": `<table><tr><th>REPOSITORY</th><th>TAG</th><th>IMAGE ID</th></tr>
            <tr><td>python</td><td>latest</td><td>0bcbeb494bed</td></tr>
            <tr><td>python</td><td>slim</td><td>23a699b503e0</td></tr>
            <tr><td>kaggle/python</td><td>latest</td><td>103a0101947e</td></tr></table>`,
    };
</script>

<Navigation bind:switchers bind:tabs />
<section class="subbody">
    <h1>Rozdział 3</h1>
    <h2>- Docker Hub -</h2>

    {#if tabs[1].active}
        <article class="content-area">
            <h2>3.1</h2>

            <h3>Docker Hub - repozytorium obrazów</h3>
            Aplikacje uruchamiane są w kontenerach (można zamiennie używać określenia
            kontener i aplikacja), te za to budowane są z obrazów. Obrazy można tworzyć
            samemu lub korzystać z tych, które są już opublikowane w internecie.
            Najłatwiej znaleźć je na największym repozytorium obrazów dockerowych
            - Docker Hubie. Przechowuje ono obrazy (wyłącznie, kontenerów tam nie
            ma), tak samo, jak np. GitHub przechowuje kod. Z Docker Huba można skorzystać
            z konsoli lub korzystając bezpośrednio ze strony:<a
                href="https://hub.docker.com/"
                target="_blank"
            >
                [link]</a
            >.

            <h3>Jak nazywane są obrazy?</h3>
            Obrazy oficjalne (stworzone przez developerów Dockera w celu stanowienia
            podstaw do budowy kolejnych obrazów, o tym w kolejnym rozdziale) mają
            w nazwach po prostu nazwę usługi, którą zawierają (jak python, java,
            ubuntu, redis, mysql, mongo itd). Obrazy opublikowane przez społeczność
            są dwuczłonowe, z czego pierwszy człon to najczęściej (ale nie musi tak
            być) nazwa użytkownika autora obrazu, a drugi człon to nazwa usługi (username/usługa).
            <br /><br />
            Ponadto używa się tagów w konwencji: username/usługa:tag. Tagi to konkretne
            wersje obrazu, np. najnowszy (w momencie pisania poradnika) oficjalny
            obraz pythona można uzyskać podając nastepującą nazwę i tag -> python:3.9.7.
            Tagi nie muszą jednak oznaczać wersjonowania oprogramowania, np. obraz
            python:slim oznacza pythona, który ma zainstalowaną minimalną liczbę
            paczek, aby sam obraz był jak najlżejszy. Można spotkać również obraz
            python:3.9.7-slim (3.9.7-slim to tag). Tag domyślny nazywa się latest,
            jest wywoływany automatycznie, gdy nie poda się tagu (a więc obraz python
            jest wczytywany automatycznie jako python:latest).

            <h3>Jak pobrać obraz z chmury?</h3>
            Najpierw można przeszukać repozytorium Docker Huba ręcznie w przeglądarce
            lub w konsoli używajać polecenia search (np. docker search python wyszuka
            wszystkie obrazy oficjalne i nieoficjalne, które mają podobne nazwy).
            Następnie można pobrać wybrany obraz komendą pull (tutaj należy podać
            już konkretną nazwę oraz tag).

            <List items={hubItems} />
        </article>
        <br />
    {:else if tabs[0].active}
        <article class="content-area">
            <h2>3.2</h2>

            <article class="instructions">
                {#if command == "docker search python"}
                    W chmurze odnaleziono następujące obrazy:
                    <article class="cubesList">
                        {#if 2}
                            <CubeNamed cubeType={cubes.image} name={"python"} />
                            <CubeNamed
                                cubeType={cubes.image}
                                name={"kaggle/python"}
                            />
                        {/if}
                    </article>
                    Można zauważyć, że pierwszy jest obrazem oficjalnym (stworzonym
                    przez developerów Dockera), a drugi posiada człon z nazwą użytkownika/organizacji,
                    więc pochodzi od społeczności. W obu przypadkach nie wyświetlamy
                    tu jednak dostępnych tagów, które można sprawdzić na stronie
                    internetowej Docker Huba. Spróbuj pobrać teraz oficjalny obraz
                    pythona używajac komendy docker pull python.
                {:else if command == "docker pull python"}
                    Pobrany obraz zawsze ma tag. Przy jego braku, docker zakłada
                    tag domyślny - latest (warto jedak pamiętać, że nazwa moze
                    być myląca i wcale nie musi to być najnowsza wersja obrazu,
                    to tylko tag domyślny, gdy nie nadano mu żadnego innego
                    tagu):
                    <article class="cubesList">
                        {#if 2}
                            <CubeNamed
                                cubeType={cubes.image}
                                name={"python:latest"}
                            />
                        {/if}
                    </article>
                    Po sprawdzeniu tagów obrazu pythona w Docker Hub wiemy jednak,
                    że jest tam m.in. tag slim. Spróbuj więc teraz pobrać inną wersję
                    tego samego obrazu (przyda się komenda docker pull python:slim).
                {:else if command == "docker pull python:slim"}
                    Teraz posiadasz na dysku już dwa obrazy (a konkretniej dwie
                    wersje tego samego obrazu):
                    <article class="cubesList">
                        {#if 2}
                            <CubeNamed
                                cubeType={cubes.image}
                                name={"python:latest"}
                            />
                            <CubeNamed
                                cubeType={cubes.image}
                                name={"python:slim"}
                            />
                        {/if}
                    </article>
                    Teraz spróbuj pobrac nieoficjalny obraz pythona z kaggle. Według
                    strony internetowej ma on tylko jeden tag (domyślny latest),
                    więc przy pobieraniu nie trzeba podawać żadnego tagu (docker
                    pull kaggle/python).
                {:else if command == "docker pull kaggle/python"}
                    Dobra robota. Na koniec wyświetl wszystkie obrazy, które
                    znajdują się lokalnie na dysku przy pomocy komendy docker
                    image ls.
                {:else if command == "docker image ls"}
                    Na dysku znajdują się poniższe obrazy (lista również w
                    terminalu). Przejdź do następnego rozdziału.
                    <article class="cubesList">
                        {#if 2}
                            <CubeNamed
                                cubeType={cubes.image}
                                name={"python:latest"}
                            />
                            <CubeNamed
                                cubeType={cubes.image}
                                name={"python:slim"}
                            />
                            <CubeNamed
                                cubeType={cubes.image}
                                name={"kaggle/python:latest"}
                            />
                        {/if}
                    </article>
                {:else}
                    Za pomocą terminala spróbuj odnaleźć w Docker Hubie obrazy z
                    pythonem (może się przydać polecenie docker search python).
                {/if}
            </article>

            <Terminal bind:command {output} />
        </article>
        <br />
    {/if}
</section>

<style>
</style>
