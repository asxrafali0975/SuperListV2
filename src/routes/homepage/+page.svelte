<script>
    import { onMount } from "svelte";
    import "../+page.svelte";
    import { goto } from "$app/navigation";
    import { MODE } from "../../store";
  
    import { arr } from "../../store";
    import TimeCompo from "../components/TimeCompo.svelte";

    let input_value = $state(null);
    let storage_arr = [];

    onMount(() => {
        let mode = localStorage.getItem("superListMode").trim();

        $MODE = parseFloat(mode);
        let token = localStorage.getItem("superlist");

        if (!token) {
            goto("/login");
        }

        let stored_tasks = localStorage.getItem("superlist_tasks");
        if (stored_tasks) {
            stored_tasks = JSON.parse(stored_tasks);
            $arr = stored_tasks;
        }
       
    });

    const addtask = () => {
        if(!input_value){
            alert("Tasks cannot be empty")
            return 
        }
        let obj = { data: input_value, status: false };
        $arr = [...$arr, obj];
        input_value = "";

        localStorage.setItem("superlist_tasks", JSON.stringify($arr));
    };

    const toggleFnc = () => {
        let tog = $MODE;
        tog = tog === 1 ? 0 : 1;
        $MODE = tog;
        localStorage.setItem("superListMode", $MODE);
    };
    let doneTasks=$state(0)

    $arr.forEach((value)=>{
        if(value.status===true)doneTasks++

    })

   
</script>

<div
    id="container"
    data-theme={$MODE ? "valentine" : "dark"}
    class="h-[100vh] w-[100vw] flex items-center justify-center flex-col"
>
    <div id="toggleBtn" class=" w-full flex items-center justify-between">
        <div class="h-[100%]  flex items-center justify-center">
           <TimeCompo/>
        </div>
        <h1 class="font-bold font-serif">Task-page</h1> 

        <label class="swap swap-rotate pr-4">
            <!-- this hidden checkbox controls the state -->
            <input type="checkbox" onclick={toggleFnc} />

            <!-- sun icon -->
            <svg
                class="swap-on h-10 w-10 fill-current"
                xmlns="http://www.w3.org/2000/svg"
                viewBox="0 0 24 24"
            >
                <path
                    d="M5.64,17l-.71.71a1,1,0,0,0,0,1.41,1,1,0,0,0,1.41,0l.71-.71A1,1,0,0,0,5.64,17ZM5,12a1,1,0,0,0-1-1H3a1,1,0,0,0,0,2H4A1,1,0,0,0,5,12Zm7-7a1,1,0,0,0,1-1V3a1,1,0,0,0-2,0V4A1,1,0,0,0,12,5ZM5.64,7.05a1,1,0,0,0,.7.29,1,1,0,0,0,.71-.29,1,1,0,0,0,0-1.41l-.71-.71A1,1,0,0,0,4.93,6.34Zm12,.29a1,1,0,0,0,.7-.29l.71-.71a1,1,0,1,0-1.41-1.41L17,5.64a1,1,0,0,0,0,1.41A1,1,0,0,0,17.66,7.34ZM21,11H20a1,1,0,0,0,0,2h1a1,1,0,0,0,0-2Zm-9,8a1,1,0,0,0-1,1v1a1,1,0,0,0,2,0V20A1,1,0,0,0,12,19ZM18.36,17A1,1,0,0,0,17,18.36l.71.71a1,1,0,0,0,1.41,0,1,1,0,0,0,0-1.41ZM12,6.5A5.5,5.5,0,1,0,17.5,12,5.51,5.51,0,0,0,12,6.5Zm0,9A3.5,3.5,0,1,1,15.5,12,3.5,3.5,0,0,1,12,15.5Z"
                />
            </svg>

            <!-- moon icon -->
            <svg
                class="swap-off h-10 w-10 fill-current"
                xmlns="http://www.w3.org/2000/svg"
                viewBox="0 0 24 24"
            >
                <path
                    d="M21.64,13a1,1,0,0,0-1.05-.14,8.05,8.05,0,0,1-3.37.73A8.15,8.15,0,0,1,9.08,5.49a8.59,8.59,0,0,1,.25-2A1,1,0,0,0,8,2.36,10.14,10.14,0,1,0,22,14.05,1,1,0,0,0,21.64,13Zm-9.5,6.69A8.14,8.14,0,0,1,7.08,5.22v.27A10.15,10.15,0,0,0,17.22,15.63a9.79,9.79,0,0,0,2.1-.22A8.11,8.11,0,0,1,12.14,19.73Z"
                />
            </svg>
        </label>
    </div>

    <div id="main-container" class="h-[95%] w-full border-2 border-black">
        <div
            id="one"
            class="h-[20%] w-full flex items-center justify-evenly border-b-2 border-b-gray-400"
        >
            <input
                id="input"
                bind:value={input_value}
                type="text"
                placeholder="Enter The Task"
                class={$MODE
                    ? "input  input-bordered  w-full max-w-xs focus:border-1 focus:border-black focus:text-black "
                    : "input  input-bordered  bg-slate-200 text-black  w-full max-w-xs "}
            />
            <button
                id="btn"
                onclick={addtask}
                class={$MODE
                    ? "btn btn-primary border-2 border-black"
                    : "btn btn-primary border-2 border-white"}>Add Task</button
            >
        </div>
        <div id="two" class="h-[80%] w-full">

            <!-- WORK IS GOING ON HERE -->
            <div
            id="two-one"
                class=" w-full h-[50%] flex items-center justify-evenly"
            >
           
            <button class="btn  btn-secondary  bg-[#FF204E]  btn-md sm:btn-lg md:btn-lg lg:btn-lg  "
            >Remaining Tasks {$arr.length-doneTasks}</button
        >

            <button class="btn btn-accent bg-[#9EC8B9]   btn-md sm:btn-lg md:btn-lg lg:btn-lg  " >Finished Tasks {doneTasks}</button>
               
            </div>
            <div
                id="two-two"
                class=" w-full h-[50%] flex items-center justify-evenly "
            >

            <button class="btn btn-accent bg-[#EEE4B1] btn-md sm:btn-lg md:btn-lg lg:btn-lg  " >Total tasks -> {$arr.length}</button>
            <button class="btn btn-warning  btn-md sm:btn-lg md:btn-lg lg:btn-lg "
                    onclick={() => goto("/viewtask")}>View Tasks</button
                >
               


            </div>
        </div>
    </div>
</div>

<style>
    @media (min-width: 280px) and (max-width: 640px) {
        #one {
            display: flex;
            align-items: center;
            justify-content: space-evenly;
            flex-direction: column;
            
        }
        #two {
            display: flex;
            align-items: center;
            justify-content: center;
            flex-direction: column;
           
        }

        #input {
            width: 80%;
        }
        #btn {
            width: 20%;
        }
    }

    #toggleBtn,
    #one {
        border-bottom: 1.5px solid rgba(214, 214, 214, 0.689);
    }
</style>
