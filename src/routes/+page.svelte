<script>
    import { page } from "$app/stores";
    import { browser } from "$app/environment";
    import RookieCard from "$lib/components/RookieCard.svelte";
    import Chapter1 from "$lib/components/Chapter1.svelte";

    let query = "";
    let result = "";

    // Auto-run when someone shares a link like /plum-candy
    $: if (browser && $page.url.pathname !== "/" && $page.url.pathname !== "") {
        query = decodeURIComponent($page.url.pathname.slice(1));
        runCanon();
    } else {
        // Reset if on root
        if (browser && $page.url.pathname === "/") {
            query = "";
            result = "";
        }
    }

    function runCanon() {
        if (!query.trim()) {
            result = "";
            return;
        }

        // ←←← YOUR CANON LOGIC GOES HERE ONLY ←←←
        const lower = query.toLowerCase().trim();
        if (
            lower === "chapter 1" ||
            lower === "chapter-1" ||
            lower === "chapter1"
        ) {
            result = "_CHAPTER1_"; // Internal signal to render component
        } else {
            result = query.toUpperCase() + " CANON";
        }
        // ←←←←←←←←←←←←←←←←←←←←←←←←←←←←←←←←←←←←←←←←←←←←←←←←←←←←←←←←←←

        if (browser) {
            history.replaceState({}, "", `/${encodeURIComponent(query)}`);
        }
    }
</script>

<div class="page-container">
    {#if result === "_CHAPTER1_"}
        <Chapter1 />
    {:else if !query}
        <RookieCard />
    {:else}
        <div class="result-container">
            <h1>{result}</h1>
        </div>
    {/if}
</div>

<style>
    .page-container {
        min-height: 100vh;
        padding: 2rem 1rem;
        background-color: #000;
    }

    .result-container {
        display: flex;
        align-items: center;
        justify-content: center;
        height: 80vh;
    }

    h1 {
        font-size: 3rem;
        font-weight: 900;
        letter-spacing: 0.1em;
        color: #fff;
        text-transform: uppercase;
    }
</style>
