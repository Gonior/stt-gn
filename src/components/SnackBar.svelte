<script lang="ts">
    import {fly} from 'svelte/transition'
    export let message:string = "This is Message"
</script>
<div class="absolute top-5 mx-auto bg-black px-2 py-1 text-white flex flex-grow-0 rounded z-10" in:fly={{y : -100, duration:300}} out:fly={{y : -100, duration:500}}>
    <p class="text-sm">{message}</p>
</div>