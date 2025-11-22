<script>
    import { fly } from "svelte/transition";
    import fallbackFacts from "$lib/data/facts.json";
    import { createEventDispatcher } from "svelte";

    // Props: facts (array), startIndex (number), onUnlock (function)
    let { facts = [], startIndex = 0, onUnlock = null } = $props();

    // Fallback if no facts passed
    if (!facts || facts.length === 0) {
        facts = fallbackFacts;
    }

    const dispatch = createEventDispatcher();

    let isUnlocked = $state(false);
    let currentIndex = $state(startIndex);
    let currentFact = $derived(facts[currentIndex]);
    let isFirst = $derived(currentIndex === 0);
    let isLast = $derived(currentIndex === facts.length - 1);

    function next() {
        if (!isLast) {
            currentIndex += 1;
            if (currentIndex === facts.length - 1) {
                isUnlocked = true;
                // Callback prop if provided
                if (typeof onUnlock === "function") {
                    onUnlock();
                }
                // Dispatch event for parent listeners
                dispatch("unlock");
            }
        }
    }

    function prev() {
        if (!isFirst) {
            currentIndex -= 1;
        }
    }
</script>

<section class="whole">
    <div class="facts">
        <!-- Prev Button -->
        <div class="controls">
            <button onclick={prev} disabled={isFirst} class="btn nav-btn"
                >&larr;</button
            >
        </div>

        <div class="science-panel">
            <div class="carousel-viewport">
                <!-- Structure of the fact -->
                {#key currentIndex}
                    <article
                        class="fact-card"
                        in:fly={{ x: 50, duration: 400, opacity: 0 }}
                        out:fly={{ x: -50, duration: 200, opacity: 0 }}
                    >
                        <div class="icon-wrapper">
                            {currentFact.icon}
                        </div>
                        <h3>{currentFact.label}</h3>
                        <div class="separator"></div>
                        <p>{currentFact.description}</p>
                    </article>
                {/key}
            </div>
        </div>
        <div>
            <!-- Next Button -->

            <div class="controls">
                <button onclick={next} disabled={isLast} class="btn nav-btn"
                    >&rarr;</button
                >
            </div>
        </div>
    </div>

    <!-- Dots. For each fact, one dot. Mark the active panel as a full dot -->
    <div class="progress-dots">
        {#each facts as _, i}
            <div class="dot" class:active={i === currentIndex}></div>
        {/each}
    </div>

    <div class="secret">
        {#if isUnlocked}
            <a href="./meetup" class="secret-btn btn"
                >Meetup with Pink Dolphin</a
            >
        {/if}
    </div>
</section>

<style>
    /*Section */
    .whole {
        display: flex;
        justify-content: center;
        flex-direction: column;
        align-items: center;
        gap: 40px;
    }
    /* Facts + Controls */
    .facts {
        display: flex;
        justify-content: center;
        align-items: center;
        gap: 20px;
        width: 100%;
    }

    /* Fact Card */
    .science-panel {
        background-color: #f7eef5;
        max-width: 50%;
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;
        padding: 50px;
        border-radius: 40px;
        box-shadow: rgb(209, 165, 180) 10px 10px 10px;
    }
    .carousel-viewport {
        position: relative;
        max-height: 1600px;
        min-height: 400px;
        display: grid;
        place-items: center;
        overflow: hidden;
    }

    .fact-card {
        grid-area: 1/1;
        text-align: left;
        padding: 20px;
    }

    .icon-wrapper {
        text-align: center;
        font-size: 4rem;
        margin-bottom: 0.5rem;
    }
    .separator {
        height: 2px;
        background: rgb(193, 193, 202);
        margin: 1rem 0;
        border-radius: 2px;
    }
    p {
        line-height: 1.6;
        color: #475569;
    }

    /* Navigation Controls */
    .controls {
        padding: 8px;
        border-radius: 100px;
        box-shadow: rgb(209, 165, 180) 5px 5px 5px;
        border-left: #f8eded solid 3px;
        background-color: rgb(238, 218, 232);
    }
    .controls:hover {
        box-shadow: rgb(204, 149, 156) 5px 5px 5px;
        border-left: #ffeded solid 3px;
        background-color: rgb(240, 200, 204);
    }

    .btn {
        padding: 10px;
        background: none;
        cursor: pointer;
        font-weight: 600;
        font-size: 2rem;
        transition: all 0.2s;
        color: rgb(141, 105, 114);
    }

    .btn:disabled {
        opacity: 0.4;
        cursor: not-allowed;
    }

    /* Dots */
    .progress-dots {
        display: flex;
        gap: 8px;
    }

    .dot {
        width: 8px;
        height: 8px;
        background: rgb(212, 175, 187);
        border-radius: 50%;
        transition: background 0.3s;
    }

    .dot.active {
        background: rgb(190, 106, 120);
        transform: scale(1.2);
    }

    /* Secret message */
    .secret-btn {
        display: inline-block;
        background: #a04b5d;
        color: white;
        text-decoration: none;
        padding: 0.75rem 1.5rem;
        border-radius: 50px;
        font-weight: bold;
        font-size: 1.3rem;
        box-shadow: 5px 4px 12px rgba(185, 4, 80, 0.25);
        transition: transform 0.2s;
    }

    .secret-btn:hover {
        transform: translateY(-2px);
        box-shadow: 0 10px 15px rgba(221, 15, 59, 0.4);
    }

    @media (max-width: 550px) {
        .facts {
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            gap: 20px;
            width: 100%;
        }
        .science-panel {
            max-width: 100%;
            padding: 10px;
        }
        .icon-wrapper {
            font-size: 2.5rem;
        }
        .fact-card p{
            padding: 5px;
            font-size: 1.15rem;
        }
    }
</style>
