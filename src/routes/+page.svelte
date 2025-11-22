<script>
    import FactsPanel from "$lib/components/facts-panel.svelte";
</script>

<main>
    <section class="hero">
        <div class="container">
            <h1>Pink Dolphins</h1>
            <p>
                The world's largest freshwater dolphin is rapidly disappearing.
                Discover the myths, the facts, and the fight to save the Inia
                geoffrensis - The Pink Dolphin
            </p>
            <a href="https/github.com/AliciaEA" class="btn">Read More</a>
        </div>
    </section>

    <section class="intro container">
        <img src="./images/pink-dolphin-intro.png" alt="Pink Dolphin" />
        <div class="text container">
            <h2>Meet the Pink Dolphin</h2>
            <p>
                Often called the <strong>Boto</strong>, the Amazon Pink River
                Dolphin is not your average marine mammal. Native to the
                freshwater basins of the Amazon and Orinoco rivers in South
                America, they are the largest and smartest of the five
                freshwaters dolphin species left on Earth.
            </p>
        </div>
    </section>

    <section class="info">
        <h2>Information</h2>
        <div class="grid">
            <div class="mini-fact">
                <span>📝</span>
                <h3>Scientific name</h3>
                <p>Inia geoffrensis</p>
            </div>
            <div class="mini-fact">
                <span>🏞️</span>
                <h3>Habitat</h3>
                <p>
                    Freshwater rivers in Brazil, Peru, Bolivia, Ecuador,
                    Colombia, and Venezuela.
                </p>
            </div>
            <div class="mini-fact">
                <span>📏</span>
                <h3>Size</h3>
                <p>
                    Up to 9 feet (2.7 meters) long and weighing up to 400 lbs.
                </p>
            </div>
            <div class="mini-fact">
                <span>⚠️</span>
                <h3>Status</h3>
                <p>Endangered (IUCN Red List).</p>
            </div>
        </div>
    </section>

    <section class="whole-paragraph">
        <h2>Why are they <span id="pink">Pink?</span></h2>
        <p>
            Believe it or not, they aren't born pink! These dolphins are born
            gray and slowly turn pink as they age. The pink coloration is
            influenced by their capillary placement, diet, and exposure to
            sunlight.
        </p>
    </section>
    <section class="grid container" id="paragraphs">
        <div class="card">
            <h3>Unmatched Agility</h3>
            <p>
                Unlike ocean dolphins, the Boto has unfused neck vertebrae. This
                allows them to turn their heads 180 degrees, helping them
                navigate through flooded forests and weave around tree trunks to
                catch prey.
            </p>
        </div>
        <div class="card">
            <h3>Master Hunters</h3>
            <p>
                They use precision echolocation to find fish in muddy, dark
                river water where visibility is zero.
            </p>
        </div>
        <div class="card">
            <h3>A Diverse Diet</h3>
            <p>
                They eat over 53 different species of fish, including piranhas,
                and even freshwater turtles.
            </p>
        </div>
    </section>

    <section class="circles container">
        <h2>The Major Threats</h2>
        <div class="grid">
            <div class="circle">
                <span class="material-symbols-outlined">skull</span>
                <h3>Mercury Pollution</h3>
            </div>
            <div class="circle">
                <span class="material-symbols-outlined">destruction</span>
                <h3>Habitat Loss</h3>
            </div>
            <div class="circle">
                <span class="material-symbols-outlined">rowing</span>
                <h3>Fishing Conflicts</h3>
            </div>
        </div>
    </section>

    <section class="facts-section container">
        <FactsPanel />

    </section>
</main>

<style>
    /* Hero Section */
    h1 {
        font-family: "Jim Nightshade";
        font-size: 3.5rem;
        color: rgb(245, 221, 232);
    }
    .hero {
        background: linear-gradient(
                rgba(207, 161, 161, 0.35),
                rgba(49, 8, 8, 0.35)
            ),
            url("/images/hero-background.jpg");
        background-size: cover;
        background-position: center;
        height: 600px;
        text-align: center;
        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: center;
    }

    .hero p {
        color: rgb(209, 186, 199);
        margin-bottom: 3rem;
    }

    /* Intro Section */
    .intro {
        display: grid;
        grid-template-columns: 600px 1fr;
        align-items: center;
        margin-top: 5rem;
    }
    .intro img {
        margin: 0 auto 20px auto;
        max-width: 800px;
        border-radius: 10px;
    }

    /* Info Section */
    .info {
        margin: 6rem 3rem;
    }
    .grid {
        display: grid;
        grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
        gap: 20px;
        margin-top: 3rem;
    }
    .mini-fact span {
        font-size: 6rem;
    }
    .mini-fact {
        background-color: rgb(226, 200, 209);
        border-radius: 50px;
        padding: 50px;
        text-align: center;
    }
    .mini-fact:hover {
        background-color: rgb(202, 174, 184);
        border-radius: 100px;
        padding: 50px;
        text-align: center;
    }

    /* Paragraphs Section */

    .whole-paragraph {
        margin-top: 10rem;
        text-align: center;
        background-color: rgb(218, 171, 181);
        padding: 100px 50px 200px 50px;
    }
    #pink {
        color: rgb(196, 27, 97);
        text-decoration: underline;
        padding: 5px 25px;
    }
    #pink:hover {
        background-color: rgb(194, 91, 134);
        border-radius: 20px;
        color: rgb(248, 182, 199);
        text-decoration: none;
    }
    #paragraphs {
        margin-top: -8rem;
    }

    .card {
        background-color: rgb(216, 121, 142);
        border-radius: 80px;
        padding: 40px;
        border: 3px rgb(139, 60, 77) solid;
    }
    .card:hover {
        background-color: rgb(238, 170, 170);
        border: 3px rgb(170, 88, 106) dotted;
    }

    /* Circles Section */

    .circles{
        margin: 10rem 0;
        text-align: center;

    }

    .circle{
        padding: 20px;
    }

    .circle span{
        background-color: rgb(224, 176, 201);
        padding: 60px;
        border-radius: 100px;
        font-size: 3rem;
        color: rgb(146, 10, 78);
        
    }

    .circle:hover span{
        font-variation-settings: 'FILL' 1, 'wght' 700, 'GRAD' 0, 'opsz' 48;
        background-color: rgb(230, 164, 164);
        color: brown;
    }
    /* Media */

    @media (max-width: 999px) {
        .intro {
            flex-direction: column;
            grid-template-columns: 1fr;
            text-align: center;
        }
        .intro img {
            max-width: 400px;
        }
    }
    @media (max-width: 450px) {
        .intro img {
            max-width: 250px;
        }
    }
</style>
