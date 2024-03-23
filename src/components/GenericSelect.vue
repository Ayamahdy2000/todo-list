<template>
  <multiselect
    v-model="state.value"
    :options="options"
    track-by="id"
    label="label"
    :placeholder="placeholder"
    :show-labels="false"
  ></multiselect>
</template>

<script>
import { reactive, watch } from "vue";
import Multiselect from "vue-multiselect";
export default {
  props: ["options","placeholder"],
  components: { Multiselect },
  emits:["sortVal"],
  setup(props,{emit}) {
    const state = reactive({
      value: null
    });
    watch(()=>state.value , ()=>{
      emit("sortVal", state.value)
    })

    return {
      state,
    };
  },
};
</script>
