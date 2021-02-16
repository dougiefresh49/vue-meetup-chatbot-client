<template>
  <div id="root">
    <Sidebar :users="state.users" />
    <Chat :messages="state.messages" @message="message" />
    <LoginModal v-if="!state.loggedIn" @login="login" />
  </div>
</template>

<script setup>
import { reactive } from "vue";

import Chat from "./components/Chat.vue";
import LoginModal from "./components/LoginModal.vue";
import Sidebar from "./components/Sidebar.vue";

import { getRandomColor } from './utils/colors';

const socket = io("https://9ufpt.sse.codesandbox.io");

socket.on("chat message", (msg) => {
  state.messages.push(msg);
});

const state = reactive({
  loggedIn: false,
  user: null,
  messages: [],
  users: [],
});

socket.on("user list updated", (users) => {
  console.log(users);
  state.users = users;
});


function login(user) {
  user.color = getRandomColor();
  socket.emit("login", user);
  state.loggedIn = true;
  state.user = user;
}

function message(message) {
  socket.emit("chat message", {
    name: `${state.user.firstName} ${state.user.lastName}`,
    time: new Date(),
    text: message,
  });
}
</script>

<style lang="postcss">
@tailwind base;
@tailwind components;
@tailwind utilities;

#root {
  @apply bg-gray-800 h-screen text-white flex flex-row;
}
</style>
