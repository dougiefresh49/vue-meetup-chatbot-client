<template>
  <div id="root">
    <div v-if="!state.connectionError">
      <Sidebar :users="state.users" />
      <Chat
        :messages="state.messages"
        @message="message"
        :loggedIn="state.loggedIn"
      />
      <LoginModal v-if="!state.loggedIn" @login="login" />
    </div>
    <div v-if="state.connectionError">
      An error occurred connecting to the chat server
    </div>
  </div>
</template>

<script setup>
import { reactive } from "vue";

import Chat from "./components/Chat.vue";
import LoginModal from "./components/LoginModal.vue";
import Sidebar from "./components/Sidebar.vue";

import { getRandomColor } from "./utils/colors";

const state = reactive({
  loggedIn: false,
  user: null,
  messages: [],
  users: [],
  connectionError: false,
});

// TODO: fill in
const SOCKET_ENDPOINT = "";
let socket;

try {
  socket = io(SOCKET_ENDPOINT);
  state.connectionError = false;
} catch (e) {
  console.error(`[ERROR] connection error: ${e.message}`);
  state.connectionError = true;
}

// TODO: fill in
// HINT: add listener for when a new user is added to update state.users

// TODO: fill in
// HINT: add listener for when a new message added to the stream

// Note: get user from previous session if available
const USER_STORAGE_KEY = "dvm:user";
const storedUser = JSON.parse(localStorage.getItem(USER_STORAGE_KEY));

if (storedUser) login(storedUser);

function login(user) {
  if (!user.color) user.color = getRandomColor();
  localStorage.setItem(USER_STORAGE_KEY, JSON.stringify(user));

  // TODO: fill in
  // HINT: tell server who is joining the chat

  state.loggedIn = true;
  state.user = user;
}

function message(message) {
  // TODO: fill in
  // HINT: tell server there is a new message
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
