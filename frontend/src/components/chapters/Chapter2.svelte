<script lang="ts">
    import Navigation from "../Navigation.svelte";
    import List from "../List.svelte";
    import Terminal from "../Terminal.svelte";

    export let switchers;

    let tabs = [
        { id: 2, active: false },
        { id: 1, active: false },
        { id: 0, active: true },
    ];

    const imageItems = [
        {
            no: 1,
            title: "docker image ls",
            content:
                "wyświetla listę obrazów, które znajdują się lokalnie na dysku, a więc których można użyć do budowy kontenerów",
        },
        {
            no: 2,
            title: "docker image inspect <id>",
            content: "wyświetla szczegóły (w tym warstwy) wskazanego obrazu",
        },
        {
            no: 3,
            title: "docker images",
            content: "działa identycznie, jak docker image ls",
        },
    ];

    const containerItems = [
        {
            no: 1,
            title: "docker container ls",
            content: "wyświetla listę URUCHOMIONYCH kontenerów",
        },
        {
            no: 2,
            title: "docker container ls -a ",
            content:
                "wyświetla wszystkie kontenery (w tym i te zatrzymane), które znajdują się lokalnie na dysku (a to skrót od all)",
        },
        {
            no: 3,
            title: "docker container inspect <id>",
            content:
                "podobnie jak w przypadku obrazów, wyświetla szczegóły dotyczące konkretnego kontenera",
        },
        {
            no: 4,
            title: "docker container logs <id>",
            content: "wyświetla logi z działania wskazanego kontenera",
        },
    ];

    let command;
    let output = {
        "docker images": `<table><tr><th>REPOSITORY</th><th>IMAGE ID</th><th>SIZE</th></tr>
            <tr><td>mongo</td><td>0bcbeb494bed</td><td>684MB</td></tr>
            <tr><td>python</td><td>23a699b503e0</td><td>338MB</td></tr>
            <tr><td>ubuntu</td><td>103a0101947e</td><td>28MB</td></tr></table>`,
        "docker image ls": `<table><tr><th>REPOSITORY</th><th>IMAGE ID  </th><th>SIZE</th></tr>
            <tr><td>mongo</td><td>0bcbeb494bed</td><td>684MB</td></tr>
            <tr><td>python</td><td>23a699b503e0</td><td>338MB</td></tr>
            <tr><td>ubuntu</td><td>103a0101947e</td><td>28MB</td></tr></table>`,
        "docker image inspect 2": `Tutaj pojawiłaby się bardzo długa lista zawierająca wszelkie szczegóły  tego konketnego obrazu (jak pełne ID, data utworzenia, konfiguracja, czy warstwy, z których zbudowany jest obraz (o tym w rozdziale Dockerfile)).`,
        "docker container ls": `<table><tr><th>CONTAINER ID  </th><th>IMAGE</th><th>NAMES</th></tr>
            <tr><td>569c9ab333d3</td><td>python</td><td>applicaton1</td></tr>
            <tr><td>569dedab27102</td><td>python</td><td>application2</td></tr></table>`,
        "docker container inspect 569c": `Tutaj, podobnie jak w przypadku inspekcji obrazów, pojawiłaby się długa lista szczegółów. Ale na tym można tu zakończyć, zapraszam do rozdziału 3!`,
    };
</script>

<Navigation bind:switchers bind:tabs />
<section class="subbody">
    <h1>Rozdział 2</h1>
    <h2>- Obraz vs kontener -</h2>

    {#if tabs[2].active}
        <article class="content-area">
            <h2>2.1</h2>

            <h3>Czym jest obraz (image)?</h3>
            Obraz to zapisany stan aplikacji i jego środowiska w danym momencie czasu
            (można więc powiedzieć, że to migawka (snapshot) aplikacji). Jest niezmienny
            (read-only) i nie można go bezpośrednio uruchomić. Zawiera kod źródłowy,
            wszelkie potrzebne biblioteki i zależności, narzędzia i wszystkie inne
            pliki potrzebne do zbudowania środowiska. Obrazy służą jako budulec do
            budowania kontenerów, które to już są uruchamiane.
            <br /><br />
            Obraz składa się z warstw (kolejnych poleceń, o tym więcej w rozdziale
            o Dockerfile). W uproszczeniu, podczas budowania obrazu następuje kopiowanie
            wszystkich warstw i dołożenie warstwy konteneru (warstwy obrazu są tylko
            do odczytu, podczas gdy warstwa konteneru jest modyfikowalna).

            <h3>Czym jest kontener?</h3>
            Kontener jest wirtualnym, izolowanym środowiskiem uruchomieniowym. Powstaje
            jako kopia obrazu, można go uruchamiać. Kontenery można porównać do maszyn
            wirtualnych, róznią się jednak tym, że w przypadku maszyn wirtualizacja
            odbywa się na poziomie hardware'u, a w przypadku kontenerów na poziomie
            aplikacji, co sprawia, że kontenery są znacznie lżejsze.

            <h3>Relacja obrazów i kontenerów</h3>
            Kontener jest budowany z obrazu. Z jednego obrazu można zbudować dowolnie
            dużo kontenerów (ale jeden kontener jest budowany zawsze z jednego obrazu),
            co umożliwia łatwe udostępnianie aplikacji oraz dużą skalowalność. Ich
            relację można porówać do obiektowości - obraz jest w tym wypadku jak
            klasa, a kontener jak obiekt stworzony na podstawie tej klasy.
        </article>
        <br />
    {:else if tabs[1].active}
        <article class="content-area">
            <h2>2.2</h2>

            <h3>Komendy do przeglądu obrazów</h3>
            <List items={imageItems} />

            Wszystkie komendy dotyczące obrazów i kontenerów zaczynają się
            zawsze od słowa kluczowego docker. W przypadku obrazów drugim słowem
            kluczowym jest image (w liczbie pojedynczej, za wyjątkiem aliasu
            images), potem trzecim słowem jest już konkretna komenda, np. ls
            (skrót od list).

            <h3>Komendy do przeglądania kontenerów</h3>
            <List items={containerItems} />

            Te komendy wyglądają podobnie, jak w przypadku obrazów. Główną
            różnicą jest słowo kluczowe container, znowu w liczbie pojedynczej
            (tym razem nie ma jednak aliasu w postaci komendy docker
            containers).

            <h3>Jak identyfikować obraz/kontener?</h3>
            W komendach dotyczących konkretnych obrazów lub kontenerów trzeba podać
            identyfikator. Można zrobić to na dwa sposoby. Pierwszym jest podanie
            ich nazwy (po użyciu komend z ls nazwa obrazu znajduje się w kolumnie
            REPOSITORY, a nazwa konteneru w kolumnie NAMES). Wygodniejszy może być
            jednak drugi sposób, czyli podanie ID. O ile samo ID jest dość skomplikowane,
            o tyle sprawa jest uproszczona, ponieważ wystarczy podać tylko jego pierwsze
            litery (tyle, ile wystarczy, aby jednoznacznie określić obraz). Poniżej
            3 przykładowe obrazy (określone nazwą i numerem ID):
            <ul>
                <li>python <strong>c</strong>2e9b18f0bca</li>
                <li>ubuntu <strong>f34</strong>4fbea3f69</li>
                <li>firefox <strong>f33</strong>9920cd560</li>
            </ul>

            Wytłuszczone zostały fragmentu ID, które wystarczą do identyfikacji
            obrazu (a więc do przejrzenia warstw obrazu ubuntu należy użyć
            polecenia docker image inspect f34). Ostatnie dwa ID wymagają
            podania więcej znaków, ponieważ oba mają identyczne dwa pierwsze
            znaki i ich podanie nie wystarczy, aby jednoznacznie sklasyfikować
            obraz.
        </article>
        <br />
    {:else if tabs[0].active}
        <article class="content-area">
            <h2>2.3</h2>

            <article class="instructions">
                {#if command == "docker images"}
                    Bardzo dobrze, lista pokazuje 3 obrazy. Wszystkie mają ID
                    rozpoczynajace się od innego znaku, więc już pierwszy znak
                    będzie wystarczajacy w każdym wypadku, aby zidentyfikować
                    konkretne obrazy. Przy użyciu polecenia docker image inspect
                    id, wyświetl szczegóły dotyczące obrazu zawierającego
                    pythona (pamiętaj, że w tym wypadku wystarczy tylko pierwszy
                    znak ID).
                {:else if command == "docker image ls"}
                    Bardzo dobrze, lista pokazuje 3 obrazy. Wszystkie mają ID
                    rozpoczynajace się od innego znaku, więc już pierwszy znak
                    będzie wystarczajacy w każdym wypadku, aby zidentyfikować
                    konkretne obrazy. Przy użyciu polecenia docker image inspect
                    id, wyświetl szczegóły dotyczące obrazu zawierającego
                    pythona (pamiętaj, że w tym wypadku wystarczy tylko pierwszy
                    znak ID).
                {:else if command == "docker image inspect 2"}
                    Dobra robota. Teraz pora zerknąć na listę kontenerów (przyda
                    się komenda docker container ls).
                {:else if command == "docker container ls"}
                    Tym razem lista zawiera 2 obiekty - są to kontenery o
                    nazwach application1 oraz application2. Jak widać, oba
                    zostały zbudowane z tego samego obrazu pythona. Używając
                    komendy docker container inspect id sprawdź szczegóły
                    pierwszego kontenera. Warto zwrócić uwagę, że w tym
                    przykładzie powtarzają się aż 3 pierwsze znaki ID (569...),
                    co oznacza, że do poprawnego zidentyfikowania kontenera,
                    potrzeba aż 4 pierwszych znaków.
                {:else if command == "docker container inspect 569c"}
                    W tym miejscu kończy się rozdział 2.
                {:else}
                    W tym miejscu na bieżąco będą aktualizowane instrukcje. Aby
                    wpisać komendę do konsoli, kliknij myszką na prawo od
                    migającego kursora i zacznij pisać. Na początek warto
                    wyświetlić listę obrazów używając komendy docker image ls
                    albo docker images.
                {/if}
            </article>
            <br /><br />
            <Terminal bind:command {output} />
        </article>
        <br />
    {/if}
</section>

<style>
</style>
