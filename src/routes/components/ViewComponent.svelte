<script>
    let prop = $props();

    import "../+page.svelte";
    let editTaskDataBind = $state(null);
    let edit_task_bool = $state(true);
    import { arr } from "../../store";

    
    const addEditTaskFunc = (index) => {
        if (!editTaskDataBind) {
            alert("task cannot be empty ");
            return;
        } else {
            $arr[index].data = editTaskDataBind;
            localStorage.setItem("superlist_tasks", JSON.stringify($arr));
            edit_task_bool = !edit_task_bool;
        }
    };
   
</script>

<button
    class={edit_task_bool ? "btn btn-outline" : "btn btn-outline hidden"}
    onclick={() => (edit_task_bool = !edit_task_bool)}>Edit</button
>
<button
class={edit_task_bool
        ? "btn btn-outline btn-accent hidden   btn-xs sm:btn-sm md:btn-sm lg:btn-md"
        : "btn btn-outline btn-accent          btn-xs sm:btn-sm md:btn-sm lg:btn-md "}
    onclick={() => addEditTaskFunc(prop.index)}>Add</button
    >
    <input
    type="text"
    
    placeholder="Enter New Task Here"
    bind:value={editTaskDataBind}
    class={edit_task_bool
        ? "input input-bordered input-error w-full max-w-xs hidden "
        : "input input-bordered input-error w-full max-w-xs  input-md sm:input-md md:input-md lg:btn-md"}
/>

<style>
    @media (min-width: 280px) and (max-width: 640px) {
    button {
        height: 10vw;
    }
   
}
</style>
