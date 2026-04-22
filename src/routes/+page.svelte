<script lang="ts">
    import UILayout from "./UILayout.svelte";
</script>

<UILayout title="UI Demo">
    {#each Array(15) as _, i}
        <div class="card">
            <p>
                Demo List Item { i + 1 }
            </p>
        </div>
    {/each}
</UILayout>

<style>
    .card {
        background-color: white;
        border-radius: 18px;
        padding: 20px 24px;
        margin-bottom: 16px;
        box-shadow: 0 2px 10px rgba(0, 0, 0, 0.03);
        font-size: 1.1rem;
        font-weight: 500;
        color: #1c1c1e;
    }
</style>
