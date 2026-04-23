<script lang="ts">
    let { title = "Title", children } = $props();
     
    let scrollY = $state(0);
    let innerHeight = $state(0);

    let contentStart = $derived(innerHeight * 0.33);
    let headerHeight = 60;
    let maxScroll = $derived(contentStart - headerHeight);

    let progress = $derived(
        (maxScroll > 0) ? Math.max(0, Math.min(scrollY / maxScroll, 1)) : 0
    );
</script>


<svelte:window bind:scrollY bind:innerHeight />


<div class="ui-container">
    <header
        class="sticky-header"
        style="
            background-color: rgba(255, 255, 255, { progress * 0.85 });
            backdrop-filter: blur({ progress * 12 }px);
            -webkit-backdrop-filter: blur({ progress * 12 }px);
            border-bottom: 1px solid rgba(0, 0, 0, { progress * 0.1 });
        "
    >
        <h1 class="small-title" style="opacity: { Math.pow(progress, 3) };">
            { title }
        </h1>
    </header>
    
    <div class="large-title-area" style="height: { contentStart }px;">
        <div
            class="large-title"
            style="
                transform: scale({ 1 - progress * 0.2 }) translateY({ scrollY * 0.35 }px);
                opacity: { Math.max(0, 1 - progress * 1.8 )};
            "
        >
            <h1>
                { title }
            </h1>
        </div>
    </div>
    
    <div class="snap-anchor"></div>

    <main class="content-area">
        {@render children()}
    </main>
</div>


<style>
    :global(html) {
        scroll-snap-type: y proximity;
        scroll-snap-stop: always;
    }

    :global(body) {
        margin: 0;
        padding: 0;
        font-family: system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;
        background-color: #f2f2f7;
        overflow-x: hidden;

        --side-margin: calc(24px + env(safe-area-inset-left));
    }

    .ui-container {
        position: relative;
        width: 100%;
    }

    .sticky-header {
        position: fixed;
        top: 0;
        left: 0;
        width: 100%;
        height: 60px;
        display: flex;
        align-items: center;
        padding: 0 var(--side-margin);
        z-index: 100;
        box-sizing: border-box;
        transition: background-color 0s;
    }

    .small-title {
        font-size: 1.25rem;
        font-weight: 550;
        color: black;
        margin: 0;
    }

    .large-title-area {
        scroll-snap-align: start;

        background-color: white;

        display: flex;
        align-items: flex-end;
        padding: 0 var(--side-margin) 16px var(--side-margin);
        box-sizing: border-box;
        position: relative;
        z-index: 10;
    }

    .large-title {
        transform-origin: left top;
        will-change: transform, opacity;
    }

    .large-title h1 {
        font-size: 2.8rem;
        font-weight: 550;
        letter-spacing: -0.02em;
        color: black;
        margin: 0;
    }

    .snap-anchor {
        scroll-snap-align: start;
        scroll-margin-top: 60px;

        height: 0;
        pointer-events: none;
    }

    .content-area {
        padding: 32px var(--side-margin) 64px var(--side-margin);
        position: relative;
        z-index: 20;

        border-top: 1px solid rgba(0, 0, 0, 0.05);
        background-color: #f2f2f7;
    }
</style>
