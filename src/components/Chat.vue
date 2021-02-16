<template>
  <main>
    <div class="message-container">
      <div
        v-for="message in props.messages"
        class="message-wrapper"
        :key="message.id"
      >
        <div class="message-from">{{ message.name }}</div>
        <div class="message-bubble">
          <div class="message-bubble-text">{{ message.text }}</div>
        </div>
      </div>
    </div>
    <form novalidate @submit="sendMessage">
      <input v-model="state.message" />
    </form>
  </main>
</template>

<script>
import { defineComponent, reactive } from "vue";

export default defineComponent({
  props: ["messages"],
  setup(props, { emit }) {
    const state = reactive({
      message: "",
      messages: [],
    });

    function sendMessage(e) {
      e.preventDefault();
      emit("message", state.message);
      state.message = "";
    }

    return {
      state,
      props,
      sendMessage,
    };
  },
});
</script>

<style scoped lang="postcss">
main {
  @apply h-full w-full flex flex-col;
}

.message-container {
  @apply p-5 flex-1 h-screen overflow-y-scroll;
}

.message-wrapper {
  @apply py-3 flex flex-col items-start;
}

.message-from {
  @apply text-purple-300 text-sm;
}

.message-bubble {
  @apply bg-gray-700 max-w-2xl w-full px-5 py-3 mt-1 rounded-full;
}

form {
  @apply p-3 w-full flex;
}

input {
  @apply flex-1 bg-gray-700 h-12 px-5 rounded border-0 outline-none rounded-lg;
}
</style>
