<script>
    import {fade} from 'svelte/transition'
    export let showModal = false;

</script>

{#if showModal}
    <div class="bg-gray-400 bg-opacity-50 min-h-screen w-full absolute flex justify-center items-center inset-0"  in:fade={{duration : 100}} out:fade={{duration : 200}}  on:click|self>
        <slot></slot>
    </div>
{/if}

<style>


</style>
