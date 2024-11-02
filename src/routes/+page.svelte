<script lang="ts">
import { socket } from "$lib/socket";


import type { Chat } from "$lib/types";
import { onMount } from "svelte";

const selfId = "3"

let chats = $state<Chat[]>([])

let text = $state("")

function send() {
    socket.emit('chat', {
        id: "0",
        userId: "3",
        userName: "Si Svelte",
        message: text,
        date: new Date(),
    })
    
    text = ""
}

onMount(() => {
    socket.on('chat', (chat: Chat) => {
        chats.push(chat)
    })
})

</script>

<div class="bg-red-400 h-full flex flex-col">
    <div class="flex-1 bg-gray-200 p-4 flex flex-col">
        {#each chats as chat}
            {#if chat.userId === selfId}
            <div class="bg-emerald-300 my-1 p-3 rounded-lg flex flex-col self-end ml-8">
                <span class="text-sm text-gray-700">Kamu</span>
                <span>{ chat.message }</span>
            </div>
            {:else}
            <div class="bg-white my-1 p-3 rounded-lg flex flex-col self-start mr-8">
                <span class="text-sm text-gray-700">{ chat.userName }</span>
                <span>{ chat.message }</span>
            </div>
            {/if}
        
        {/each}
    </div>
    <div class="flex">
        <input
            class="flex-1 p-2 outline-none"
            type="text"
            bind:value={text}
        />
        
        <button
            onclick={send}
            class="bg-blue-500 px-3 text-white"
        >
            Send
        </button>
    </div>
</div>