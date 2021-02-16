<template>
  <h1>Test</h1>
</template>

<script>
import { computed, defineComponent } from "vue";
import SidebarUser from "./SidebarUser.vue";

export default defineComponent({
  props: ["users"],
  components: { SidebarUser },
  setup(props) {
    const usersWithAcronyms = computed(() => {
      return props.users.map((u) => ({
        ...u,
        acronym: `${u.firstName[0].toUpperCase()}${u.lastName[0].toUpperCase()}`,
      }));
    });

    return {
      props,
      usersWithAcronyms,
    };
  },
});
</script>

<style lang="postcss">
.avatar {
  @apply rounded-full h-10 w-10 flex items-center justify-center bg-red-400 font-bold mr-3;
}

.user {
  @apply flex items-center py-2;
}

nav {
  @apply w-1/4 h-full p-3 bg-gray-900;
}
</style>
