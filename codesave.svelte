<script>
    import '../+page.svelte'
    import { arr } from '../../store';
    
    
    const finished=(index)=>{
        $arr[index].status=!$arr[index].status;
        localStorage.setItem("superlist_tasks", JSON.stringify($arr));
    
    }
    const deletetask=(index)=>{
        let dummy = $arr
        dummy.splice(index,1)
        $arr=dummy
        localStorage.setItem("superlist_tasks", JSON.stringify($arr));
    
    
    }
    
    </script>
    
    
    <div class="bg-slate-800 h-[1000000vh] w-[100vw] p-3">
        <h1 class="center bg-blue-600 font-serif h-[0.0007%]" >Tasks Lists</h1>
    
    {#each $arr as value, index}
    <div class="border"> 
        <h1  class={value.status ?  " done bg-slate-400 m-2 font-serif " : "bg-slate-400 m-2 font-serif " } > {index}->{value.data}</h1>
        <button class="bg-green-300 rounded-md " onclick={()=>finished(index)}>{ value.status ? "Undone" :"Done"  }</button>
        <button class="bg-red-300 rounded-md"  onclick={()=>deletetask(index)}>Delete</button>
    </div>
    
    {/each}
    
    </div>
    
    
    <style>
        h1{
            font-size: 3vw;
            border: 1.5px solid black;
            border-radius: 5px;
            
        }
        
        .border{
            border: 2px solid black;
            margin-bottom:14px;
        }
    
        .center{
            display: flex;
            align-items: center;
            justify-content: center;
            color: white;
            margin-bottom: 30px;
            font-weight: 600;
            
        }
    
        .done{
            background-color: rgb(153, 235, 160);
            text-decoration: line-through;
        }
        
        
    </style>