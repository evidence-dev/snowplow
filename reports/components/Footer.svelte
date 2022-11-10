<script>
    import BigLink from "@evidence-dev/components/ui/BigLink.svelte";
    export let prev;
    export let next;
</script>

<div class="footer">
    {#if prev}<BigLink href={prev}>&laquo; Prev Page</BigLink>{:else}<div></div>{/if}
    {#if next}<BigLink href={next}>Next Page &raquo;</BigLink>{:else}<div></div>{/if}
</div>

<style>
    .footer {
        display: flex;
        justify-content: space-between;
        margin-top: 2rem;
    }   
</style>

