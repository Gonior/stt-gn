<script lang="ts">
	import {Router } from 'svelte-routing'
	import NavLink from './NavLink.svelte'
    
	export let url:string;
	url = "";
</script>
<style lang="postcss">
</style>    

<Router url={url} >
    <div class="w-64 bg-gray-100 p-4 shadow-lg">
        <div class="flex flex-row items-center space-x-1 mb-4">
            <svg class="icon multi-color" width="48" height="48" id="package-alt" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24"><title style="stroke-width: 2;">package alt</title><polygon id="secondary-fill" points="20 7.5 20 16.5 12 21 12 11.5 20 7.5" style="fill: rgb(255, 137, 0); stroke-width: 2;"></polygon><polygon id="tertiary-fill" points="20 7.5 12 11.5 4 7.5 12 3 20 7.5" style="fill: rgb(136, 0, 255); stroke-width: 2;"></polygon><path id="primary-stroke" d="M12,11.5V21L4,16.5v-9ZM12,3,4,7.5l8,4,8-4Zm0,8.5V21l8-4.5v-9ZM16,14V9.5l-4-2" style="fill: none; stroke: rgb(0, 0, 0); stroke-linecap: round; stroke-linejoin: round; stroke-width: 2;"></path></svg>
            <h1 class="text-2xl font-sans font-semibold">Rekap-gn</h1>
        </div>
        <nav class="flex flex-col space-y-4">
            <NavLink to='/'>
                <div class="flex items-center justify-between py-2 px-4 hover:bg-gray-200 rounded-lg text-gray-400">
                    <span>Home</span>
                    <span id="dot" class="hidden rounded-full h-2 w-2 bg-purple-600"></span>
                </div>
            </NavLink>
            <NavLink to="employee">
                <div class="flex items-center justify-between py-2 px-4 hover:bg-gray-200 rounded-lg text-gray-400">
                    <span>Employee</span>
                    <span id="dot" class="hidden rounded-full h-2 w-2 bg-purple-600"></span>
                </div>
            </NavLink>
        </nav>
    </div>
	<div class="w-full">
        <slot />
    </div>
</Router>

