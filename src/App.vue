<template>
  <div class="container todo">
    <header class="d-md-flex justify-content-between header">
      <div>
        <h1 class="header__title">My Tasks</h1>
        <p class="hedaer__subTitle">
          Manage your habits, reminders, events, activites.
        </p>
      </div>
      <button class="generic-btn d-md-block d-flex ms-auto align-items-center" @click="state.showTask = true">
        <i class="icon-plus"></i> New Task
      </button>
    </header>
    <main>
      <add-task @addTask="addTask" @closeAddTask="closeAddTask" v-if="state.showTask"/>
      <generic-tabs v-if="state.tasks.length > 0" @getTab="getTab">
        <generic-select
          :options="state.options"
          placeholder="Sort By"
          @sortVal="sortVal"
        />
        <div class="row">
          <div
            class="col-lg-6 col-md-12"
            v-for="(task, index) in state.tasks"
            :key="index"
          >
            <task-card
              :task="task"
              :index="index"
              @isCompleted="getStatus"
              @deleteTask="deleteTask"
            />
          </div>
        </div>
      </generic-tabs>
      <empty-state
        v-if="state.tasks.length == 0 && !state.showTask"
        image="Add-tasks"
        note="Start adding tasks to stay organized and productive!"
        text="Your to-do list is currently empty"
        btn="Add a task"
        @showTaskSec="showTaskSec"
      />
    </main>
    <generic-toast
      :failed="state.failed"
      @closed="closed"
      :toasterText="state.text"
      :show="state.isShow"
    />
  </div>
</template>

<script>
import { reactive } from "vue";
import TaskCard from "./components/CardTask.vue";
import GenericToast from "./components/GenericToast.vue";
import EmptyState from "./components/EmptyState.vue";
import GenericTabs from "./components/GenericTabs.vue";
import GenericSelect from "./components/GenericSelect.vue";
import AddTask from "./components/AddTask.vue";
export default {
  name: "App",
  components: {
    TaskCard,
    AddTask,
    EmptyState,
    GenericTabs,
    GenericToast,
    GenericSelect,
  },
  setup() {
    const state = reactive({
      tasks: localStorage.myTasks ? JSON.parse(localStorage.myTasks) : [],
      options: [
        {
          id: 0,
          label: "Sort By A to Z",
        },
        {
          id: 1,
          label: "Sort By Z to A",
        },
        {
          id: 2,
          label: "Sort By Ascending order",
        },
        {
          id: 3,
          label: "Sort By Descending order",
        },
      ],
      failed: false,
      text: "",
      isShow: false,
      showTask: false
    });
    const getStatus = (val) => {
      state.tasks[val.index].isComplete = val.isComplete;
      localStorage.myTasks = JSON.stringify(state.tasks);
    };
    const sortVal = (val) => {
      if (val.id == 0) {
        state.tasks = state.tasks.sort(function (a, b) {
          return a.title.localeCompare(b.title);
        });
      } else if (val.id == 1) {
        state.tasks = state.tasks.sort(function (a, b) {
          return b.title.localeCompare(a.title);
        });
      } else if (val.id == 2) {
        state.tasks = state.tasks.sort(function (a, b) {
          return a.id - b.id;
        });
      } else {
        state.tasks = state.tasks.sort(function (a, b) {
          return b.id - a.id;
        });
      }
    };
    const deleteTask = (index) => {
      state.tasks.splice(index, 1);
      state.isShow = true;
      state.text = "Task deleted successfully.";
      localStorage.myTasks = JSON.stringify(state.tasks);
    };
    const closed = () => {
      state.isShow = false;
    };
    const closeAddTask = () =>{
      state.showTask = false
    }
    const getTab = (val) => {
      let tasks = localStorage.myTasks ? JSON.parse(localStorage.myTasks) : [];
      if (val == 1) {
        state.tasks = tasks;
      } else if (val == 2) {
        state.tasks = tasks.filter((el) => el.isComplete == true);
      } else {
        state.tasks = tasks.filter((el) => !el.isComplete);
      }
    };
    const addTask = (val) => {
      let length = state.tasks.length;

      state.tasks.push({
        id: state.tasks[length - 1] ? state.tasks[length - 1].id + 1 : 0 ,
        isComplete: false,
        desc: val.desc,
        title: val.title,
        date: val.date,
      });
      localStorage.myTasks = JSON.stringify(state.tasks)
    };
    const showTaskSec = () =>{
      state.showTask = true
    }
    return {
      state,
      getStatus,
      showTaskSec,
      closeAddTask,
      deleteTask,
      closed,
      getTab,
      sortVal,
      addTask,
    };
  },
  mounted() {
    this.state.tasks = localStorage.myTasks? JSON.parse(localStorage.myTasks) : [];
  },
};
</script>
