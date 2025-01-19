<script>
    import { onMount } from 'svelte';
    import '../+page.svelte'
    import { goto } from '$app/navigation';

import { arr } from '../../store';
    
        let input_value = $state(null);
        let storage_arr=[];
       
    
    onMount(()=>{
        if(!localStorage.getItem("superlist")){
            goto('/login')

        }
         
        let stored_tasks=localStorage.getItem("superlist_tasks")
        if(stored_tasks){
            stored_tasks=JSON.parse(stored_tasks)
             $arr=stored_tasks
        }
    })

    const addtask=()=>{
        let obj={data:input_value,status:false}
        $arr=[...$arr,obj]
        input_value=""

        localStorage.setItem("superlist_tasks", JSON.stringify($arr));
        


    
    }


</script>

<div id="container" class="bg-slate-600 h-[100vh] w-[100vw]">
    <div id="one" class="bg-slate-950 h-[30%] w-[100%] flex items-center justify-around">
        <input type="text" placeholder="Enter your task" bind:value={input_value} class=" h-[23%] font-medium rounded-md w-[68%] focus:border-2  focus:border-black" >
        <button class="btn bg-blue-300 h-[23%] rounded-md p-2 flex items-center justify-center" onclick={addtask}>Add Task</button>

    </div>
    <div id="two" class="flex h-[70%] flex-col ">
        <div id="one" class="mid bg-yellow-400 w-[100%] h-[50%] font-semibold  ">Total tasks {$arr.length} </div>
        <div id="two" class="mid bg-red-400 w-[100%] h-[50%]" onclick={()=>goto('/viewtask')}>View tasks</div>
    </div>


</div>


<style>
    .btn{
        font-size: 2vmin;

    }

    .mid{
        display: flex;
        align-items: center;
        justify-content: center ;
        font-size: larger;
    }

</style>