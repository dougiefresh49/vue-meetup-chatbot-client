<template>
  <div class="user">
    <span class="avatar">{{ acronym }}</span>
    {{ props.user.firstName }} {{ props.user.lastName }}
  </div>
</template>

<script>
import { createComponent } from "vue";

export default createComponent({
  props: ["user"],
  setup(props) {
    const acronym = `${props.user.firstName[0].toUpperCase()} ${props.user.lastName[0].toUpperCase()}`;
    return {
      acronym,
      props,
    };
  },
});
</script>