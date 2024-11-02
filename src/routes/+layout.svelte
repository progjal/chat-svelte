<script lang="ts">

import { socket } from "$lib/socket";
import "../app.css";
import { onDestroy, onMount } from "svelte";

function onMessage(msg: string) {
    console.log(`Message : "${msg}"`)
}

onMount(() => {
    socket.on('message', onMessage)
})

onDestroy(() => {
    socket.off('message', onMessage)
})

</script>

<slot />