<script>
    import "../app.css";
    import { goto } from "$app/navigation";
    import { onMount } from "svelte";
    let username = null;
    let password = null;

    onMount(() => {
        goto("/login");
    });
</script>
