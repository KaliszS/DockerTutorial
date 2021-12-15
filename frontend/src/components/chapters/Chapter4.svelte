<script lang="ts">
    import Navigation from "../Navigation.svelte";
    import CubeNamed from "../CubeNamed.svelte";
    import List from "../List.svelte";
    import Editor from "../Editor.svelte";
    import Cloud from "../Cloud.svelte";
    import DragDrop from "../DragDrop.svelte";

    export let switchers;

    const cubes = {
        image: "image",
        container: "container",
    };

    let tabs = [
        { id: 3, active: false },
        { id: 2, active: false },
        { id: 1, active: false },
        { id: 0, active: true },
    ];

    const code = `<span style="color: #008afa;">FROM</span> python:3.9.7 <br /><br>
        <span style="color: #008afa;">COPY</span> ./requirements.txt /app/requirements.txt <br />
        <span style="color: #008afa;">RUN</span> pip install -r /app/requirements.txt <br />
        <span style="color: #008afa;">COPY</span> ./src /app/src <br /><br>
        <span style="color: #008afa;">CMD</span> python app/src/main.py <br />`;

    const codeBackend = `<span style="color: #707070;"># ./backend/Dockerfile</span><br /><br>
    <span style="color: #008afa;">FROM</span> tiangolo/uvicorn-gunicorn-fastapi:latest <br /><br>
        <span style="color: #008afa;">COPY</span> ./requirements.txt /app/requirements.txt <br />
        <span style="color: #008afa;">RUN</span> pip install -r /app/requirements.txt <br />
        <span style="color: #008afa;">COPY</span> ./src /app/src <br /><br>
        <span style="color: #008afa;">CMD</span> uvicorn run:app --host 0.0.0.0 --port 80 <br />`;

    const codeFrontend = `<span style="color: #707070;"># ./frontend/Dockerfile</span><br /><br>
    <span style="color: #008afa;">FROM</span> node:current-alpine <br /><br>
    <span style="color: #008afa;">COPY</span> package*.json . <br />
    <span style="color: #008afa;">RUN</span> npm install <br />
    <span style="color: #008afa;">COPY</span> /src ./src <br /><br>
    <span style="color: #008afa;">CMD</span> npm run dev <br />`;

    const runItems = [
        {
            no: 1,
            title: "docker build -t nazwa ścieżka",
            content:
                "Buduje obraz z Dockerfile (-t nazwa określa nazwę obrazu, ściezka to ścieżka do Dockerfile, jeśli uruchamiamy komendę z tej samej lokalizacji, co Dockerfile, wystarczy podać kropkę, np. 'docker build -t user/my_python .' .",
        },
        {
            no: 2,
            title: "docker run nazwa_obrazu",
            content:
                "Uruchamia kontener z obrazu (którego nazwę podajemy), można dodać parametr --name, by podać własną nazwę kontenera, inaczej Docker wygeneruje losową.",
        },
    ];

    let visibility = {
        im1: false,
        im2: false,
        im3: false,
        im4: false,
        co1: false,
        co2: false,
        co3: false,
        co4: false,
    };

    function showCube(id) {
        visibility[id] = true;
    }
</script>

<Navigation bind:switchers bind:tabs />
<section class="subbody">
    <h1>Rozdział 4</h1>
    <h2>- Dockerfile -</h2>

    {#if tabs[3].active}
        <article class="content-area">
            <h2>4.1</h2>
            <h3>Dockerfile</h3>

            Obrazy można pozyskiwać z Docker Huba, ale również można zbudować
            własne. Instrukcje do zbudowania obrazu zapisuje się w plikach o
            nazwie Dockerfile (nie mają one rozszerzeń). Załóżmy przykładową
            strukturę projektu: katalog src (a w nim skrypty pythona, czyli
            pliki z rozszerzeniem py), plik Dockerfile oraz plik
            requirements.txt (zawierający listę bibliotek do pythona, które
            chcemy zainstalować na użytek projektu). Plik Dockerfile składa się
            z kolejnych instrukcji, przykładowo takich, jak poniżej. Warto tylko
            najpierw zaznaczyć, że to jest tylko "przepis" na obraz. Większość
            instrukcji zostanie uruchomiona przy budowaniu obrazu, za wyjątkiem
            ostatniej, która uruchamia się dopiero, gdy z obrazu uruchamiany
            jest kontener.
            <br /><br />
            <section class="dockerfile">
                <Editor {code} />
                <aside>
                    <list
                        ><li>
                            <strong>FROM</strong> - to powinna być zawsze pierwsza
                            instrukcja w Dockerfile, określa obraz bazowy, na podstawie
                            którego zostanie zbudowany nowy obraz (coś musi być pierwsze,
                            dlatego własnie developerzy Dockera stworzyli obrazy
                            oficjalne, można jednak jako obrazów bazowych używać
                            dowolnych obrazów nieoficjalnych). W tym przykładzie
                            używamy obrazy python o tagu 3.9.7. Obraz ten zawiera
                            całe linuxowe środowisko z zainstalowanym pythonem oraz
                            potrzebnymi do jego funkcjonowania bibliotekami (oraz
                            narzędziami, jak pip). Od tego momentu powstaje wirtualne
                            środowisko i wszystkie kolejne instrukcje będą wywoływane
                            wewnątrz niego, a nie w systemie hosta.
                        </li>
                        <li>
                            <strong>COPY</strong> - kopiuje potrzebne pliki do wirtualnego
                            środowiska, pierwszym argumentem jest ścieżka do plików
                            w systemie hosta (./requirements oraz ./src -> kropka
                            oznacza katalog, gdzie aktualnie się znajdujemy, w tym
                            wypadku, w którym znajduje się plik Dockerfile, bo z
                            niego wywoływane sa instrukcje; katalog src oraz plik
                            requirements.txt znajdują się w tym samym katalogu, co
                            Dockerfile), a drugim ścieżka wewnątrz przyszłego kontenera/wirtualnego
                            środowiska (w tym wypadku podajac ściezkę /app/... tworzymy
                            jednocześnie w środku katalog /app i do niego kopiujemy
                            wszystkie potrzebne pliki)
                        </li>
                        <li>
                            <strong>RUN</strong> - uruchamia komendę w konsoli wewnątrz
                            wirtualnego środowiska (w tym wypadku używamy narzędzia
                            pip do instalacji potrzebnych bibliotek do pythona ze
                            skopiowanego wcześniej pliku requirements.txt)
                        </li>
                        <li>
                            <strong>CMD</strong> - (skrót od Command) działa podobnie
                            do RUN, za wyjątkiem tego, że RUN można uruchomić wielokrotnie,
                            a CMD powinno być użyte raz, na końcu. Dzieje się tak,
                            ponieważ RUN uruchamia komendy w trakcie budowy obrazu,
                            a CMD uruchamia się dopiero przy uruchamianiu kontenera
                            z tego obrazu. W tym przykładzie następuje wywołanie
                            skryptu main.py, który to znajduje się w skopiowanym
                            wcześniej katalogu src. Tutaj więc powinien zostać uruchomiony
                            główny program aplikacji bądź proces podtrzymujący serwer.
                        </li></list
                    >
                </aside>
            </section>
            <br /><br />
            W skrócie powyższy Dockerfile to instrukcja do stworzenia nowego obrazu
            na podstawie obrazu python:3.9.7, która zakłada skopiowanie wszystkich
            potrzebnych plików do środka wirtualnego środowiska, zainstalowanie potrzebnych
            bibliotek oraz uruchomienie pliku main.py, który to powinien być głównym
            programem.

            <h3>Warstwy obrazu</h3>
            Obrazy zazwyczaj potrafią się budować dosyć długo. Dzieje się tak, ponieważ
            pod spodem mają najczęściej systemy linux (ale bez jądra systemowego,
            bo na tym opiera się konteneryzacja, że jest wykorzystywane jądro systemu
            hosta) i np. obraz python:3.9.7 różni się od obrazu mongo:latest tym,
            że pierwszy posiada system (np. Ubuntu) z zainstalowanym oprogramowaniem
            pythona, a drugi może mieć ten sam system, ale z oprogramowaniem do uruchomienia
            bazy danych mongodb. Oczywiście można stworzyć obraz na podstawie obrazu
            mongo i przy uzyciu instrukcji RUN zainstalować w środku pythona, ale
            byłobby to bardzo nieoptymalne rozwiązanie, ponieważ później cała aplikacja
            mieściłaby się w jednym kontenerze. Lepiej przygotować oddzielny obraz
            (a w konsekwencji kontener) na program w pythonie i oddzielny na bazę
            danych, po czym oba kontenery ze sobą połączyć (o tym w kolejnych rozdziałąch).

            <br /><br />
            Jeśli chcemy zmienić coś w Dockerfile, to będzie to wymagało ponownego
            zbudowania obrazu. Tutaj jednak przychodzą z pomocą warstwy. Dockerfile
            w trakcie budowy obrazu dokłada do niego kolejne instrukcje, które są
            warstwami (a więc najpierw obraz powstaje z jedną warstwą - FROM, potem
            w powyższym przykładzie są dodawane warstwy COPY, RUN, COPY i CMD). Jeśli
            zmienimy jedno z poleceń (lub dodamy nowe), zostanie nadpisana tylko
            warstwa zmieniona oraz wszystkie, które były dołożone po niej. Oznacza
            to, że modyfikacje obrazu ładują się szybciej (chyba, że modyfikujemy
            warstwę FROM, wtedy i tak cały obraz musi się zbudować od zera).
        </article>
        <br />
    {:else if tabs[2].active}
        <article class="content-area">
            <h2>4.2</h2>

            <h3>Jakich komend użyć?</h3>
            Najpierw należy zbudować obraz z pliku Dockerfile, a następnie uruchomić
            kontener z obrazu.

            <List items={runItems} />

            Kontener zostaje utworzony przy pierwszym uruchomieniu (docker run),
            jednak potem można go zamknąć i przechowywać, tak jak obraz.
            Wyłączony kontener będzie miał status exited, a działający running.

            <h3>Jak powstaje obraz i kontener</h3>
            Są dwie ścieżki, aby otrzymać kontener (czyli działającą aplikację):
            <ol>
                <li>
                    DockerHub --(docker pull)--> Obraz na dysku --(docker
                    run)--> Działajacy kontener
                </li>
                <li>
                    Dockerfile --(docker build)--> Obraz na dysku --(docker
                    run)--> Działajacy kontener
                </li>
            </ol>
        </article>
        <br />
    {:else if tabs[1].active}
        <article class="content-area">
            <h2>4.3</h2>
            Poniżej mała wizualizacja do wyklikania, by zobaczyć zależności pomiędzy
            obrazami, a kontenerami oraz różne ścieżki ich pozyskiwania. Do dyspozycji
            mamy chmurę DockerHub oraz dwa pliki Dockerfile (jeden do backendu, drugi
            do frontendu).

            <section class="dockerfile">
                <Cloud />
                <Editor code={codeBackend} />
                <Editor code={codeFrontend} />
            </section>

            Przy pomocy poniższych przycisków pobierz obraz z DockerHub oraz
            zbuduj dwa kolejne z plików Dockerfile.<br /><br />

            <section class="dockerfile">
                <button on:click={() => showCube("im1")}
                    >docker pull mongo</button
                >
                <button on:click={() => showCube("im2")}
                    >docker build -t user/python ./backend
                </button>
                <button on:click={() => showCube("im3")}
                    >docker build -t user/node ./frontend</button
                >
            </section>

            <article class="cubesList">
                {#if visibility.im1}
                    <CubeNamed cubeType={cubes.image} name={"mongo:latest"} />
                {/if}
                {#if visibility.im2}
                    <CubeNamed
                        cubeType={cubes.image}
                        name={"user/python:latest"}
                    />
                {/if}
                {#if visibility.im3}
                    <CubeNamed
                        cubeType={cubes.image}
                        name={"user/node:latest"}
                    />
                {/if}
            </article>

            Następnie uruchom jeden kontener z mongo oraz dwa kontenery z obrazu
            pythona (jak widać, z jednego obrazu można uruchamiać kilka
            kontenerów).<br /><br />

            <section class="dockerfile">
                <button on:click={() => showCube("co1")}
                    >docker run mongo:latest</button
                >
                <button on:click={() => showCube("co2")}
                    >docker run user/python
                </button>
                <button on:click={() => showCube("co3")}
                    >docker run user/python</button
                >
            </section>

            <article class="cubesList">
                {#if visibility.co1}
                    <CubeNamed
                        cubeType={cubes.container}
                        name={"flamboyant_goldstine"}
                    />
                {/if}
                {#if visibility.co2}
                    <CubeNamed
                        cubeType={cubes.container}
                        name={"electic_mclean"}
                    />
                    <div />
                {/if}
                {#if visibility.co3}
                    <CubeNamed
                        cubeType={cubes.container}
                        name={"elated_buck"}
                    />
                {/if}
            </article>

            Gdy sami nie nadamy nazwy kontenerom, zrobi to za nas docker i jak
            widać... są one dosyć egzotyczne.
        </article>
        <br />
    {:else if tabs[0].active}
        <article class="content-area">
            <h2>4.4</h2>
            Na koniec tego rozdziału ułóż obraz z kolejnych warstw z Dockerfile,
            przeciągajac i upuszczając kafelki z dołu. Z lewej strony powinny być
            pierwsze warstwy, z prawej ostatnie. Pamiętaj, aby najpierw użyć obrazu
            bazowego, potem skopiować pliki, następnie zainstalować biblioteki, a
            dopiero na końcu uruchomić główny skrypt.<br /><br />

            <DragDrop />

            Z gotowego obrazu (metoda przeciągnij i upuść symulowała tutaj
            komendę docker build) należy uruchomić obraz. <br /><br />

            <section class="dockerfile">
                <button on:click={() => showCube("co4")}
                    >docker run user/calc</button
                >
            </section>

            <article class="cubesList">
                {#if visibility.co4}
                    <CubeNamed cubeType={cubes.container} name={"calculator"} />
                {/if}
            </article>

        </article>
        <br />
    {/if}
</section>

<style>
    .numbers {
        background: linear-gradient(to bottom, #ffb73265 83%, red 17%);
        background-attachment: fixed;
        -webkit-text-stroke-color: #ffb73265;
        -webkit-text-fill-color: transparent;
        -webkit-background-clip: text;
        font-size: 8vw;
        font-weight: bold;
        text-align: center;
    }

    .dockerfile {
        display: flex;
        flex-flow: row wrap;
        justify-content: center;
    }

    aside {
        border: 2px solid;
        padding: 5px;
        width: 65%;
    }

    .cubesList {
        justify-content: space-around;
    }

    .content-area {
        padding-bottom: 150px;
    }
</style>
