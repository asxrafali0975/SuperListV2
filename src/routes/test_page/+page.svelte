<script>


	let doneTasks = $state(0)
	let undone=$state(0)

	array.forEach((v)=>{
		if(v.status==true)done++

	})
	

</script>


<main>
<div>
	{#each array as ar,index}
	
	<h1>{ar.data}</h1>


	{/each}

	<h1>total done tasks are {done }</h1>
	<h1>total undone task {array.length-done}</h1>
</div>

</main>