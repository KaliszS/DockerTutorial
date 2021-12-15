<script lang="ts">
    import Navigation from "../Navigation.svelte";
    import Image from "../Image.svelte";
    import Container from "../Container.svelte";
    import VM from "../VM.svelte";
    export let switchers;

    let tabs = [
        { id: 1, active: false },
        { id: 0, active: true },
    ];
</script>

<Navigation bind:switchers bind:tabs />
<section class="subbody">
    <h1>Rozdział 1</h1>
    <h2>- Konteneryzacja -</h2>

    {#if tabs[1].active}
        <article class="content-area">
            <h2>1.1</h2>

            <h3>Na czym polega wirtualizacja?</h3>
            By opisać konteneryzację, warto zacząć najpierw od pokrewnego i nieco
            starszego pojęcia wirtualizacji. Polega to na symulowaniu obecności wirtualnych
            zasobów, które to polega na tym, że procesom logicznym przydzielane są
            fizyczne, rzeczywiste zasoby komputera hosta. W wyniku wirtualizacji
            zostają uruchamiane wirtualne maszyny.Ważną zaletą wirtualizacji jest
            możliwość izolacji aplikacji wraz z procesami i danymi pomiędzy poszczególne
            maszyny wirtualne, co zwiększa bezpieczeństwo.
            <br /><br />
            Charakteryzuje się to jednak tym, że każda maszyna wirtualna posiada
            swój cały, niezależny ekosystem, w tym i system operacyjny. Sprawia to,
            że taka maszyna pochłania dużo zasobów hosta, co uwydatnia się tym bardziej,
            gdy pojawia się potrzeba wirtualizacji kilku podobnych środowisk, z których
            każde musi mieć oddzielnie instalowany cały system operacyjny. Im większa
            skala, tym mniej optymalne zużycie zasobów i mniejsza wygoda przy instalacji
            i konfiguracji środowisk.

            <h3>Na czym polega konteneryzacja?</h3>
            Konteneryzacja w uproszczeniu działa podobnie do wirtualizacji, trochę
            jak jej lżejsza wersja. Różnia polega głównie na tworzeniu kontenerów
            zamiast maszyn wirtualnych. Kontenery są lżejsze i wydajniejsze, ponieważ
            współdzielą jądro Linuxa z system operacyjnym hosta (dlatego można je
            uruchamiać tylko na Linuxach). Taki podział pozwala na dużą skalowalność,
            ponieważ operuje się tutaj na dużo lżejszych paczkach, niż wirtualne
            maszyny, co pozwala na łatwą rozbudowę całego potrzebnego ekosystemu.
            Inną różnicą jest mniejsza izolacja, niż w przypadku maszyn wirtualnych,
            co może być zarówno wadą (mniejsze bezpieczeństwo), jak i zaletą (kontenery
            mogą się dzięki temu ze sobą komunikować, na co nie można sobie pozwolić
            podczas użytkowania maszyn wirtualnych).

            <h3>Czym jest Docker?</h3>
            Docker jest platformą do uruchamiania kontenerów. Działa podobnie jak
            hypervisor (hipernadzorca, np. VirtualBox) w przypadku maszyn wirtualnych,
            w przeciwieństwie do którego nie musi jednak tworzyć wirtualnych systemów
            operacyjnych. Jest to bardzo popularne rozwiązanie do konteneryzacji,
            ale nie jedyne. Przykładem innej popularnej platformy tego typu jest
            Kubernetes.
        </article>
        <br />
    {:else if tabs[0].active}
        <article class="content-area">
            <h2>1.2</h2>
            <h3>Maszyny wirtualne</h3>
            Poniższe animacje przedstawiają uproszczony schemat budowy maszyn wirtualnych
            oraz kontenerów. Poniżej szare sześciany symbolizują maszyny wirtualne.
            Ich rozmiar jest znaczący, ponieważ zawierają w sobie całe systemy operacyjne.
            Wszystkie 3 maszyny wymagają nadal zewnętrznego hardware'u hosta oraz
            platformy hipernadzorcy.

            <div class="system">
                <div class="vm"><VM /><VM /><VM /></div>
                <div class="hyper">Hypervisor (np. VirtualBox)</div>
                <div class="os">Infrastruktura (hardware)</div>
            </div>

            <h3>Kontenery</h3>
            Kolejna wizualizacja dotyczy kontenerów serwowanych za pomoą platformy
            Docker. Sam Docker zastępuje tutaj hipernadzorcę, a kontenery są znacząco
            lżejsze/mniejsze (fioletowe sześciany), ponieważ nie instalują jądra
            systemu. Zamiast tego wykorzystują system operacyjny hosta (a to oznacza
            dodatkową warstwę po stronie środowiska hosta).

            <div class="system">
                <div class="vm"><Container /><Container /><Container /></div>
                <div class="hyper">Docker</div>
                <div class="os">System operacyjny</div>
                <div class="os">Infrastruktura (hardware)</div>
            </div>
        </article>
        <br />
    {/if}
</section>

<style>
    .system {
        width: 80%;
        margin: 20px auto;
    }
    .vm {
        display: flex;
        justify-content: center;
        margin-bottom: 10px;
    }

    .hyper,
    .os {
        text-align: center;
        margin: 6px;
        padding: 5px;
        border: 1px solid #3c6997;
    }
</style>
