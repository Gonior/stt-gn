<svelte:head>
    <title>Employee</title>
</svelte:head>
<script lang='ts'>
	
	import {onMount} from 'svelte'
	import Table from '../components/Table.svelte'
    import Pagination from '../components/Pagination.svelte'
    import Modal from '../components/Modal.svelte'
    import Form from '../components/Form.svelte'
    let dataEmployees = []
    let resp = 'No one Employee'
    const getEmployee = async ():Promise<void> => {
        let res = await fetch('http://localhost:8080/employees')

        if (res.ok) {
            let json = await res.json()
            dataEmployees = [...json]
        } 
    }

    
	let dataPerPage:number = 6
	let currentPage:number = 1
	let indexOfLast:number = currentPage*dataPerPage
	let indexOfFirst:number = indexOfLast - dataPerPage
	$: currentData= dataEmployees.slice(indexOfFirst,indexOfLast)
    
    let getPage:any;
    let showModal:boolean = false;
    let search:string = ''
    

	onMount(() => {
	 	getPage = (e:any):void => {
            currentPage = e.detail.num
            indexOfLast = currentPage*dataPerPage
            indexOfFirst = indexOfLast - dataPerPage
            currentData = dataEmployees.slice(indexOfFirst,indexOfLast)
        }	
        getEmployee()
	})
	const reFetch = (e:any):void => {
        showModal = e.detail.showModal
        getEmployee();
    }
	const handleSearch = ():void => {
		if (search !== "") searchEmployee(search)
		else currentData = dataEmployees
	}
	const searchEmployee = (params):void => {
		if (isNaN(+params)) {
			let namee = params
			currentData = dataEmployees.filter((n) => n.name.indexOf(namee) !== -1)
        } 
        else 
		{
			let id = +params
			currentData = dataEmployees.filter((n) => n.id == id)
		}
    }
    const toogleModal = () => {
        showModal = !showModal
    }
</script>

<div class="h-screen w-full">
    <div class="flex flex-col h-full">
        <header class="bg-gray-200 p-4 flex justify-between shadow-xl">
            <div>
                <input type="text" class="py-2 px-4 rounded-l-md shadow-md focus:outline-none w-80" placeholder="Search employee ID or Name" bind:value={search} on:keyup={handleSearch}>
            </div>
            <button class="py-2 px-4 font-bold rounded bg-purple-300 shadow hover:bg-purple-400" on:click="{toogleModal}">Add Employee</button>
        </header>
        <div class="p-5 flex flex-col">
            <div class="hidden sm:flex-1 sm:flex sm:items-center sm:justify-between mb-2">
                <div>
                    <p class="font-semibold text-gray-700">Number of Employees
                        <span class="font-bold">{dataEmployees.length}</span>
                    </p>
            </div>
        <div>
            {#if (dataEmployees.length > 0)}
                <Pagination totalData={dataEmployees.length} dataPerPage={dataPerPage} {currentPage} on:currentPage={getPage} />
            {/if}
        </div>
        </div>
			{#if (currentData.length > 0)}
    	        <Table employees={currentData} on:showModal={reFetch} />
			{:else}
			    <h1 class="w-full text-center text-2xl">
				    {resp}
			    </h1>
			{/if}
        </div>
    </div>
</div>
<Modal {showModal} >
    <Form on:showModal={reFetch}/>
</Modal>