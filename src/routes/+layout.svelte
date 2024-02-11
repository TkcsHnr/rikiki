<script lang="ts">
    import '../app.css';
</script>

<main>
    <header>
        <p>ri<span>kiki</span></p>
    </header>
    <slot></slot>
</main>

<style lang="scss">
    main {
        display: flex;
        flex-direction: column;
        align-items: center;
        gap: 1rem;
        padding: 2rem;

    }

    header {
        display: flex;
        width: 100%;
        p {
            font-size: 1.5rem;
            font-weight: bold;

            span {
                color: oklch(var(--p));
            }
        }
    }
</style>