<script>
    import '../+page.svelte'
    let minutes =$state(0)
    let second=$state(0)
    let hours=$state(0)
  
    setInterval(() => {
      if(second==59){
        minutes++;
      second=0;
  
    }
    if(minutes==59){
      hours++;
      minutes=0;
      
      
    }
    second++;
    
  }, 1000);
    
  </script>
  
  
  <main>
    <div class="flex items-center justify-center mt-32">
      <h1>{hours}</h1>:
      <h1>{minutes}</h1>:
      <h1>{second}</h1>
  
    </div>
  </main>