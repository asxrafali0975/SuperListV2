<script>
    import { onMount } from "svelte";


  
        let mydate = $state(new Date().toLocaleTimeString().split(":"));

       let  hour = $state(mydate[0])
   
       let  minute = $state(mydate[1])
   
       let  second=$state(mydate[2])
    
       second=second.split(" ")
       second=second[0]
  
           setInterval(() => {
            second++
            if(second==60){
                minute++
                second=0
            }
            if(minute==60){
                hour++
                minute=0
               
            }
            
           }, 1000);


   


	

</script>


<main>
<div class="text-xs ml-5  mt-3 sm:text-md  md:text-md lg:text-lg  xl:text-md  ">
	 {hour} : {minute} : {second}
</div>

</main>