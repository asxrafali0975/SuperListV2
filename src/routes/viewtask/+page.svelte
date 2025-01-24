<script>
    // importts
    import "../+page.svelte";
    import { onMount } from "svelte";
    import { goto } from "$app/navigation";
    import { MODE } from "../../store";
    import { arr } from "../../store";
    import ViewComponent from "../components/ViewComponent.svelte";
    import TimeCompo from "../components/TimeCompo.svelte";

    // variables

    let start_drag_value = null;
    let start_index = null;
    let end_drag_value = null;
    let end_index = null;

    //functions

    onMount(() => {
        let token = localStorage.getItem("superlist");
        if (!token) {
            goto("/login");
        }
        let mode = localStorage.getItem("superListMode").trim();
        $MODE = parseFloat(mode);
        let dummy = localStorage.getItem("superlist_tasks");
        if (dummy) {
            dummy = JSON.parse(dummy);
            $arr = dummy;
        }
    });

    const finished = (index) => {
        $arr[index].status = !$arr[index].status;
        localStorage.setItem("superlist_tasks", JSON.stringify($arr));
    };
    const deletetask = (index) => {
        let dummy = $arr;
        dummy.splice(index, 1);
        $arr = dummy;
        localStorage.setItem("superlist_tasks", JSON.stringify($arr));
    };

    let toggle = $state(true);

    const toggleFnc = () => {
        let tog = $MODE;
        tog = tog === 1 ? 0 : 1;
        $MODE = tog;
        localStorage.setItem("superListMode", $MODE);
    };

    const dragStart = (index) => {
        start_drag_value = $arr[index];
        start_index = index;
       
    };
    const dragTarget = (index,event) => {
        end_drag_value = $arr[index];
        end_index = index;
    

    };
    const change = () => {
        $arr[end_index] = start_drag_value;
        $arr[start_index] = end_drag_value;
        localStorage.setItem("superlist_tasks", JSON.stringify($arr));
    };
</script>

<div
    id="container"
    data-theme={$MODE ? "valentine" : "dark"}
    class="h-[1000vh] w-[100vw]"
>
    <div id="toggleBtn" class=" w-full flex items-center justify-between">
        <div class="h-[100%] flex items-center justify-center">
            <TimeCompo />
        </div>
        <h1 class="font-bold font-serif">Task-View</h1>

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

    {#each $arr as obj, index}
        <!-- svelte-ignore a11y_no_static_element_interactions -->
        <div
            class="join join-vertical w-full my-1"
            draggable="true"
            ondragstart={() => dragStart(index)}
            ondragenter={() => dragTarget(index)}
            ondragend={change}
        >
            <div
                class="collapse collapse-arrow join-item border-base-300 border"
            >
                <input type="radio" name="my-accordion-4" checked="checked" />
                <div class="collapse-title text-xl font-small font-mono">
                    Task {index + 1}
                </div>
                <div class="collapse-content">
                    <h2
                        class={obj.status
                            ? " sm:text-4xl  done  m-2 font-serif font-semibold tracking-wide"
                            : " m-2 sm:text-4xl font-serif font-semibold tracking-wide "}
                    >
                        {obj.data}
                    </h2>
                    <button
                        class="btn btn-xs sm:btn-xs md:btn-sm lg:btn-md bg-green-500"
                        onclick={() => finished(index)}
                        >{obj.status ? "Undone" : "Done"}</button
                    >
                    <button
                        class="btn btn-xs sm:btn-xs md:btn-sm lg:btn-md bg-red-500"
                        onclick={() => deletetask(index)}>Delete</button
                    >
                    <ViewComponent {index} />
                </div>
            </div>
        </div>
    {/each}
</div>

<style>
    .dragging{
        background-color: #f07474; /* Example highlight color */
    }
    #toggleBtn {
        border-bottom: 1.5px solid black;
    }

    .done {
        text-decoration: line-through;
    }

    @media (min-width: 280px) and (max-width: 640px) {
        button {
            height: 10vw;
        }
    }
</style>
