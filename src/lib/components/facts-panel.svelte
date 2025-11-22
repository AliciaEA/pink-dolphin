<script>
    import { fly, fade } from "svelte/transition";
    import scienceFacts from "$lib/data/facts.json";

    let isUnlocked = $state(false);

    // The # Slide we are on
    let currentIndex = $state(0);

    //Data based on the index
    let currentFact = $derived(scienceFacts[currentIndex]);

    let isFirst = $derived(currentIndex === 0);
    let isLast = $derived(currentIndex === scienceFacts.length - 1);

    // Next Logic Button
    function next() {
        if (!isLast) {
            currentIndex += 1;
        } else {
            isUnlocked = true;
        }
    }

    // Prev Logic Button
    function prev() {
        if (!isFirst) {
            currentIndex -= 1;
        }
    }
</script>

<section class="science-panel">
    <div class="panel-header"></div>

    <!-- Structure of the fact -->
    <div class="carousel-viewport">
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

    <!-- Prev Button -->
    <div class="controls">
        <button onclick={prev} disabled={isFirst} class="btn nav-btn"
            >&larr;</button>
    </div>

    <!-- Dots. For each fact, one dot. Mark the active panel as a full dot -->
    <div class="progress-dots">
        {#each scienceFacts as _, i}
        <div class="dot" class:active = {i === currentIndex}></div>
        {/each}
    </div>

    <!-- Next Button -->
     <!-- Prev Button -->
    <div class="controls">
        <button onclick={next} disabled={isLast} class="btn nav-btn"
            >&rarr;</button>
    </div>

    <div class="secret">
        {#if isUnlocked}
        <a href="/meetup" class="secret-btn btn">Meetup with Pink Dolphin</a>
        {/if}
    </div>
</section>
