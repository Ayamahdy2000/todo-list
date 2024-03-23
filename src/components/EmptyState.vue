<template>
  <div class="empty-state">
    <img
      v-if="image"
      :src="require(`@/assets/images/${image}.svg`)"
      :alt="image"
    />
    <h4 v-if="text">{{ text }}</h4>
    <p v-if="note">{{ note }}</p>
    <button class="generic-btn generic-btn--main-color" @click="showTaskSec" v-if="btn">
      <i class="icon-plus"></i> {{ btn }}
    </button>
  </div>
</template>
<script>
export default {
  props: ["image", "text", "btn", "note"],
  emits: ["showTaskSec"],
  setup(props, { emit }) {
    const showTaskSec = () => {
      emit("showTaskSec");
    };
    return {
      showTaskSec,
    };
  },
};
</script>
