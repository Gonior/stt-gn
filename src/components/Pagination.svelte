<script>
	export let totalData;
	export let dataPerPage;
	export let currentPage = 1
	import { createEventDispatcher, onMount } from 'svelte';
	const dispatch = createEventDispatcher();

	function nextPage() {
		dispatch('currentPage', {
			num: currentPage+1
		});
	}
	function prevPage() {
		dispatch('currentPage', {
			num: currentPage-1
		});
	}
	let getPage;
	onMount(() => {
		getPage = (num) => {
			dispatch('currentPage', {
				num: num
			});
		}	
	})
	
	
	const dataNumbers = []
	let i = 1
	while(i <= Math.ceil(totalData/dataPerPage)) {
		dataNumbers.push(i)	
		i++
	}
	
	
</script>

<nav class="relative z-0 inline-flex shadow-sm -space-x-px" aria-label="Pagination">
	{#if (currentPage > 1)}
        <button class="relative inline-flex items-center px-2 py-2 rounded-l-md border border-gray-300 bg-white text-sm font-medium text-gray-500 hover:bg-gray-50"
                                    on:click={prevPage}>
            <span class="sr-only">Previous</span>
            <!-- Heroicon name: chevron-left -->
            <svg class="h-5 w-5" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 20 20" fill="currentColor" aria-hidden="true">
            <path fill-rule="evenodd" d="M12.707 5.293a1 1 0 010 1.414L9.414 10l3.293 3.293a1 1 0 01-1.414 1.414l-4-4a1 1 0 010-1.414l4-4a1 1 0 011.414 0z" clip-rule="evenodd" />
            </svg>
        </button>
	{/if}
	{#each dataNumbers as num}
        <button class="relative inline-flex items-center px-4 py-2 border border-gray-300 bg-white text-sm font-medium {num == currentPage ? 'text-gray-700' : 'text-gray-300'} hover:bg-gray-50" on:click={getPage(num)}>
            {num}
        </button>
	{/each}
	{#if (currentPage< Math.ceil(totalData/dataPerPage))}
        <button class="relative inline-flex items-center px-2 py-2 rounded-r-md border border-gray-300 bg-white text-sm font-medium text-gray-500 hover:bg-gray-50" on:click={nextPage}>
              <span class="sr-only">Next</span>
              <svg class="h-5 w-5" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 20 20" fill="currentColor" aria-hidden="true">
                <path fill-rule="evenodd" d="M7.293 14.707a1 1 0 010-1.414L10.586 10 7.293 6.707a1 1 0 011.414-1.414l4 4a1 1 0 010 1.414l-4 4a1 1 0 01-1.414 0z" clip-rule="evenodd" />
              </svg>
        </button>
    {/if}
</nav>