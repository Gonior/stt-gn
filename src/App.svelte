<script lang='ts'>
	import Nav from './components/Nav.svelte';
	import home from './pages/home.svelte'
    import Employee from './pages/employee.svelte'
	import {Route} from 'svelte-routing'

</script>

<div class="flex flex-row h-screen">
	<Nav>
		<Route path="employee" component="{Employee}" location="{'*'}"/>
        <Route path="/" component="{home}" location="{'*'}" />
	</Nav>
</div>


<style></style>