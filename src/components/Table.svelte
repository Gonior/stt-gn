<script >
    import Modal from './Modal.svelte'
    import Form from './Form.svelte'
    import SnackBar from './SnackBar.svelte'
    import {fly} from 'svelte/transition'
    import {createEventDispatcher} from 'svelte'

    let dispatch = createEventDispatcher();

    export let employees
    let showModal = {
        edit : false,
        remove : false
    }
    
    let data = {}
    let successRemove = {
        success : false,
        message : ''
    }
    const handleEdit = (e) => {
        data = {...e}
        showModal.edit = !showModal.edit
    }
    const handleRemove = (e) => {
        data = {...e}
        showModal.remove = !showModal.remove
    }

    const removeEmployee = async (id) => {
        
        let res = await fetch(`http://localhost:8080/employees/delete/${id}`, {
            method : 'POST',
            headers : {
                    'Content-type' : 'application/json'
                }
            }
        )
        console.log(res)
        if (res.ok) 
        {
            let json = await res.json()
            successRemove.success = true
            successRemove.message = json.message
            showModal.remove = false;
            dispatch('showModal', {
                showModal : false
            })
            setTimeout(() => {
                successRemove = {
                    success : false,
                    message : ''
                }
            }, 2000)

        } else {
            let json = await res.json()
            successRemove.success = true
            successRemove.message = json.message
            showModal.remove = false;
            setTimeout(() => {

                successRemove = {
                    success : false,
                    message : ''
                }
            }, 2000)
        }
    }
</script>


<table class="shadow-lg bg-white w-full rounded">
    <tr>
        <th class="bg-purple-300 text-left px-8 py-4">ID</th>
        <th class="bg-purple-300 text-left px-8 py-4">NAME</th>
        <th class="bg-purple-300 text-left px-8 py-4">Salary</th>
        <th class="bg-purple-300 text-left px-8 py-4">Date Of Entry</th>
        <th class="bg-purple-300 text-left px-8 py-4">Action</th>
    </tr>
    {#each employees as e}
        <tr>
            <td class="border px-8 py-4">{e.id}</td>
            <td class="border px-8 py-4">{e.name}</td>
            <td class="border px-8 py-4">{e.salary}</td>
            <td class="border px-8 py-4">{e.doe.substring(0,10)}</td>
            <td class="border px-8 py-4">
                <div class="flex items-center justify-around flex-none">
                    <svg on:click="{handleEdit(e)}" class="m-0" width="14" height="14" id="edit-alt" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24"><path d="M20.41,7.83,7.24,21H3V16.76L16.17,3.59a1,1,0,0,1,1.42,0l2.82,2.82A1,1,0,0,1,20.41,7.83Z" style="fill: none; stroke: rgb(0, 72, 197); stroke-linecap: round; stroke-linejoin: round; stroke-width: 2;"></path></svg>
                    <svg on:click="{handleRemove(e)}"class="m-0" width="14" height="14" id="delete-alt" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24"><path d="M4,7H20M16,7V4a1,1,0,0,0-1-1H9A1,1,0,0,0,8,4V7" style="fill: none; stroke: rgb(198, 0, 0); stroke-linecap: round; stroke-linejoin: round; stroke-width: 2;"></path><path d="M6,7H18a0,0,0,0,1,0,0V20a1,1,0,0,1-1,1H7a1,1,0,0,1-1-1V7A0,0,0,0,1,6,7Z" style="fill: none; stroke: rgb(198, 0, 0); stroke-linecap: round; stroke-linejoin: round; stroke-width: 2;"></path><line x1="10" y1="11" x2="10" y2="17" style="fill: none; stroke: rgb(198, 0, 0); stroke-linecap: round; stroke-linejoin: round; stroke-width: 2;"></line><line x1="14" y1="11" x2="14" y2="17" style="fill: none; stroke: rgb(198, 0, 0); stroke-linecap: round; stroke-linejoin: round; stroke-width: 2;"></line></svg>
                </div>
            </td>
        </tr>
    {/each}
</table>
<Modal showModal={showModal.edit} >
    <Form on:showModal={(e) => showModal.edit = e.detail.showModal} on:showModal {...data} editEmployee={true}/>
</Modal>
<Modal showModal={showModal.remove}>
    <div class="bg-white rounded h-1/4 items-center justify-center p-5 shadow-lg space-y-6 flex flex-col" in:fly={{y:-300, duration : 300}} out:fly={{y:-300, duration : 300}}>
        <h1 class="text-lg font-semibold">Are you sure want to remove ID {data.id}?</h1>
        <div class="flex justify-around flex-row w-full">
            <button on:click={() => showModal.remove = !showModal.remove} class="py-2 px-4 rounded hover:border-gray-500 border">
                cancel
            </button>
            <button on:click={removeEmployee(data.id)} class="py-2 px-4 rounded bg-red-400 hover:bg-red-600 text-white">
                remove
            </button>
        </div>
    </div>
</Modal>
{#if successRemove.success}
    <SnackBar message="{successRemove.message}" />
{/if}