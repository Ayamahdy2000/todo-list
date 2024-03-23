<template>
  <div class="task d-flex justify-content-between" >
    <div class="d-flex">
      <div class="form-check">
        <input
          class="form-check-input"
          type="checkbox"
          v-model="state.isComplete"
          :id="`task${index}`"
        />
      </div>
      <div class="task__content">
        <h3>{{ task.title }}</h3>
        <p class="task__desc">{{ task.desc }}</p>
        <p class="task__date">{{ handleDate }}</p>
      </div>
    </div>
    <div class="task__actions" @click="deleteTask"><i class="icon-delete"></i></div>
  </div>
</template>
<script>
import dayjs from "dayjs";
import { reactive, computed, watch } from "vue";
let isToday = require("dayjs/plugin/isToday");
let isTomorrow = require("dayjs/plugin/isTomorrow");

dayjs.extend(isTomorrow);
dayjs.extend(isToday);
export default {
  props: ["task", "index"],
  emits: ["isCompleted", "deleteTask"],
  setup(props, { emit }) {
    const state = reactive({ isComplete: props.task.isComplete });
    const handleDate = computed(() => {
      if (dayjs(props.task.date).isToday()) {
        return "Today";
      } else if (dayjs(props.task.date).isTomorrow()) {
        return "Tomorrow";
      } else {
        return dayjs(props.task.date).format("MMM DD");
      }
    });
    const deleteTask = () => {
      emit("deleteTask", props.index);
    };
    watch(
      () => props.task,
      () => {
        state.isComplete = props.task.isComplete;
      }
    );
    watch(
      () => state.isComplete,
      () => {
        emit("isCompleted", {
          index: props.index,
          isComplete: state.isComplete,
        });
      }
    );
    return {
      state,
      handleDate,
      deleteTask,
    };
  },
};
</script>
