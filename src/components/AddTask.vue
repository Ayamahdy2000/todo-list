<template>
  <div class="add-form">
    <header class="d-flex justify-content-between">
      <h3>Add New Task</h3>
      <i class="icon-exit" @click="closeAddTask"></i>
    </header>
    <main>
      <div class="row">
        <div class="col-lg-6 col-md-12 mb-4">
          <input
            type="text"
            v-model="state.form.title"
            @blur="v.form.title.$touch"
            :class="v.form.title.$error ? 'generic-input--error' : ''"
            placeholder="Title"
            class="generic-input"
          />
          <div class="generic-error" v-if="v.form.title.$error">
            {{ v.form.title.$errors[0].$message }}
          </div>
        </div>
        <div class="col-lg-6 col-md-12 mb-4">
          <input
            type="text"
            v-model="state.form.desc"
            @blur="v.form.desc.$touch"
            :class="v.form.desc.$error ? 'generic-input--error' : ''"
            placeholder="Description"
            class="generic-input"
          />
          <div class="generic-error" v-if="v.form.desc.$error">
            {{ v.form.desc.$errors[0].$message }}
          </div>
        </div>
        <div class="col-lg-6 col-md-12 mb-4">
          <date-picker v-model="state.form.date">
            <template #default="{ inputValue, inputEvents }">
              <BaseInput :value="inputValue" v-on="inputEvents" />
              <input
                type="text"
                @blur="v.form.date.$touch"
                :class="v.form.date.$error ? 'generic-input--error' : ''"
                :value="inputValue"
                v-on="inputEvents"
                placeholder="Date"
                class="generic-input"
              />
            </template>
          </date-picker>
          <div class="generic-error" v-if="v.form.date.$error">
            {{ v.form.date.$errors[0].$message }}
          </div>
        </div>
        <div class="col-lg-6 col-md-12 mb-4">
          <button class="generic-btn generic-btn--main-color" @click="submit">
            Add
          </button>
        </div>
      </div>
    </main>
  </div>
</template>
<script>
import { DatePicker } from "v-calendar";
import { reactive, computed } from "vue";
import useVuelidate from "@vuelidate/core";
import { required, helpers } from "@vuelidate/validators";
export default {
  components: { DatePicker },
  emit: ["closeAddTask"],
  setup(props, { emit }) {
    const state = reactive({
      form: {
        date: "",
        title: "",
        desc: "",
      },
    });
    const rules = computed(() => {
      return {
        form: {
          title: {
            required: helpers.withMessage("Tilte is required", required),
          },
          desc: {
            required: helpers.withMessage("Description is required", required),
          },

          date: {
            required: helpers.withMessage("Date is required", required),
          },
        },
      };
    });
    const closeAddTask = () => {
      emit("closeAddTask");
    };
    const v = useVuelidate(rules, state);
    return {
      state,
      v,
      closeAddTask,
    };
  },
  methods: {
    submit() {
      this.v.$validate();
      if (!this.v.$error) {
        this.$emit("addTask", this.state.form);
        this.state.form = {}
        this.v.$reset()
      }
    },
  },
};
</script>
