<script setup>
import { Chat } from "@chat-ui/vue3";
function handleMessage(message) {
    console.log(message)
}
</script>

<template>
    <Chat :chat="[]" :onSend="handleMessage" />
</template>