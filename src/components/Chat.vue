<template>
  <main>
    <div class="message-container" ref="containerEl">
      <ChatMessage v-for="message in props.messages" :message="message" :key="message.id"/>
    </div>
    <form novalidate @submit="sendMessage">
      <input v-model="state.message" />
    </form>
  </main>
</template>

<script>
import { defineComponent, reactive, ref, onMounted, watchEffect, computed } from "vue";
import ChatMessage from './ChatMessage.vue';

export default defineComponent({
  props: ["messages"],
  components: { ChatMessage },
  setup(props, { emit }) {
    const containerEl = ref(null);

    const state = reactive({
      message: "",
      messages: [],
    });

    watchEffect(() => {
      if (containerEl.value && props.messages.length) {
        // TODO: not working
        console.log('scrolling...');
        containerEl.value.scrollIntoView(false);
      }
    });

    function sendMessage(e) {
      e.preventDefault();
      emit("message", state.message);
      state.message = "";
    }

    return {
      containerEl,
      props,
      sendMessage,
      state,
    };
  },
});
</script>

<style scoped lang="postcss">
.message-container {
  @apply p-5 flex-1 h-screen overflow-y-scroll;
}

main {
  @apply h-full w-full flex flex-col;
}

form {
  @apply p-3 w-full flex;
}

input {
  @apply flex-1 bg-gray-700 h-12 px-5 rounded border-0 outline-none rounded-lg;
}
</style>
