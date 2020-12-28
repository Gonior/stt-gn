<script lang="ts">
    import {createEventDispatcher} from 'svelte'

    import SnackBar from './SnackBar.svelte'

    const dispatch = createEventDispatcher();

    let isLoading:boolean = false;
    let errors = {
        id : "",
        name : "",
        salary : "",
        doe : ""
    }
    export let id = "";
    export let name = "";
    export let salary = "";
    export let doe = "";
    export let editEmployee = false;

    if (editEmployee) doe = doe.substr(0,10)
    let messageSuccess = '';
    let success = false;


    const handleClose = () : void => {
        if (!isLoading) {
            dispatch("showModal", {
                showModal : false
            })
        }
    }
    
    const clear = ():void => {
        id = "";
        name = "";
        salary = "";
        doe = "";
        errors = {
                id : "",
                name : "",
                salary : "",
                doe : ""
            }
    }

    const addEmployee = async () : Promise<void> => {
        let url:string;
        id = id.toString()
        salary = salary.toString()
        if (editEmployee) url = "http://localhost:8080/employees/update"
        else url = "http://localhost:8080/employees/add"
        let res = await fetch(url,{
            method: 'POST',
            headers: {
                    'Content-type': 'application/json; charset=UTF-8',
                },
            body : JSON.stringify({name, id, salary, doe})
            }
        )

        if (res.status == 200) {
            let json = await res.json()
            isLoading = false
            success = true
            messageSuccess = json.message
           
            clear();
            setTimeout(() => {
                success = false
                messageSuccess = ""
            }, 2000)
        } else {
            
            let json = await res.json()
            errors = {...json}
            isLoading = false
        }
        

    }  
    
    const handlesubmit = ():void => {
        isLoading = true
        addEmployee()
        
    }

</script>

<style>

</style>



{#if isLoading} 
    <SnackBar message="Sending Data ..." />
{/if}
{#if success} 
    <SnackBar message="{messageSuccess}" />
{/if}
<form class="flex flex-col p-10 w-1/2 rounded shadow bg-white" on:reset|preventDefault="{clear}" on:submit|preventDefault="{handlesubmit}">
    <div class="flex items-center justify-between">
        <h1 class="font-semibold text-xl text-black mb-2 tracking-wide">{editEmployee ? 'Edit Employee' : "Add Employee"}</h1>
        <div class="h-5 w-5">        
            <svg on:click={handleClose} viewBox="0 0 24 24"><line x1="19" y1="19" x2="5" y2="5" style="fill: none; stroke: rgb(125, 0, 224); stroke-linecap: round; stroke-linejoin: round; stroke-width: 2;"></line><line x1="19" y1="5" x2="5" y2="19" style="fill: none; stroke: rgb(125, 0, 224); stroke-linecap: round; stroke-linejoin: round; stroke-width: 2;"></line></svg>    
        </div>
    </div>
    <hr>
    <div class="flex flex-col space-y-1 text-black mb-2">
        <label for="idEmployee" class="block text-sm font-medium ">
            ID
        </label>
        <input type="text" readonly={editEmployee} id="idEmployee" class="focus:ring-purple-400 focus:outline-none focus:ring-2 focus:border-purple-400 border block w-full py-2 pl-1 {(errors.id == '') || (errors.id == undefined)  ? 'border-gray-300' : 'border-red-700' }  rounded-md" placeholder="123" bind:value="{id}">
        <small class="text-red-500 text-xs">{errors.id != undefined ? errors.id : ""}</small> 
    </div>
    <div class="flex flex-col space-y-1 text-black mb-2">
        <label for="name" class="block text-sm font-medium ">
            NAME
        </label>
        <input type="text" id="name" class="focus:ring-purple-400 focus:ring-2 focus:outline-none focus:border-purple-400 border block w-full py-2 pl-1 rounded-md {(errors.name == '') || (errors.name == undefined)  ? 'border-gray-300' : 'border-red-700' } " placeholder="Dedi Cahya" bind:value="{name}">
        <small class="text-red-500 text-xs">{errors.name != undefined ? errors.name : ""}</small> 
    </div>
    <div class="flex flex-col space-y-1 text-black mb-2">
        <label for="salary" class="block text-sm font-medium ">
            SALARY
        </label>
        <input type="text" id="salary" class="focus:ring-purple-400 focus:ring-2 focus:outline-none focus:border-purple-400 border block w-full py-2 pl-1 rounded-md {(errors.salary == '') || (errors.salary == undefined)  ? 'border-gray-300' : 'border-red-700' } " placeholder="1000000" bind:value="{salary}">
        <small class="text-red-500 text-xs">{errors.salary != undefined ? errors.salary : ""}</small> 
    </div>
    <div class="flex flex-col space-y-1 text-black mb-2">
        <label for="doe" class="block text-sm font-medium ">
            Date Of Entry
        </label>
        <input type="date" id="doe" class="focus:ring-purple-400 focus:ring-2 focus:border-purple-400 focus:outline-none border block w-full py-2 pl-1 rounded-md {(errors.doe == '') || (errors.doe == undefined)  ? 'border-gray-300' : 'border-red-700' } " bind:value="{doe}">
        <small class="text-red-500 text-xs">{errors.doe != undefined ? errors.doe : ""}</small> 
    </div>
    <hr>
    <div class="flex justify-end space-x-3 mr-2 items-center mt-2">
        <button type="reset" disabled={editEmployee || isLoading} class="disabled:opacity-50  py-1 px-4 text-white bg-red-400 hover:bg-red-500 rounded-md uppercase">clear</button>
        <button type="submit" disabled={isLoading} class="disabled:opacity-50 py-1 px-4 text-white bg-purple-400 hover:bg-purple-500 rounded-md uppercase">{editEmployee ? "edit" : 'add'}</button>
    </div>
</form>


