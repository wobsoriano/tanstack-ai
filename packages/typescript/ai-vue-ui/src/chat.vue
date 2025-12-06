<script setup lang="ts">
import { useChat, type UIMessage } from '@tanstack/ai-vue'
import type { ChatProps } from './types'
import { provide } from 'vue'
import { CHAT_KEY } from './use-chat-context'

const props = defineProps<ChatProps>()

const emit = defineEmits<{
  response: [response?: Response]
  chunk: [chunk: any]
  finish: [message: UIMessage]
  error: [error: Error]
}>()

defineSlots<{
  default(): any
}>()

const chat = useChat({
  connection: props.connection,
  initialMessages: props.initialMessages,
  id: props.id,
  body: props.body,
  onResponse: (response?: Response) => emit('response', response),
  onChunk: (chunk: any) => emit('chunk', chunk),
  onFinish: (message: UIMessage) => emit('finish', message),
  onError: (error: Error) => emit('error', error),
  tools: props.tools,
})

provide(CHAT_KEY, chat)
</script>

<template>
  <div :class data-chat-root>
    <slot />
  </div>
</template>
