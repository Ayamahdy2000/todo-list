<template>
  <ul class="nav nav-tabs" id="myTabs" role="tablist">
    <li class="nav-item" role="presentation">
      <button
        class="nav-link active"
        id="all-tab"
        data-bs-toggle="tab"
        data-bs-target="#all"
        type="button"
        role="tab"
        aria-controls="all"
        aria-selected="true"
   
      >
        All
      </button>
    </li>
    <li class="nav-item" role="presentation">
      <button
        class="nav-link"
        id="completed-tab"
        data-bs-toggle="tab"
        data-bs-target="#completed"
        type="button"
        role="tab"
        aria-controls="completed"
        aria-selected="false"
       
      >
        Completed
      </button>
    </li>
    <li class="nav-item" role="presentation">
      <button
        class="nav-link"
        id="inCompleted-tab"
        data-bs-toggle="tab"
        data-bs-target="#inCompleted"
        type="button"
        role="tab"
        aria-controls="inCompleted"
        aria-selected="false"
      
      >
        Incompleted
      </button>
    </li>
  </ul>
  <div class="tab-content" id="myTabContent">
    <div
      class="tab-pane fade show active"
      id="all"
      role="tabpanel"
      aria-labelledby="all-tab"
    >
      <slot ></slot>
    </div>
    <div
      class="tab-pane fade"
      id="completed"
      role="tabpanel"
      aria-labelledby="completed-tab"
    >
      <slot :data="true"></slot>
    </div>
    <div
      class="tab-pane fade"
      id="inCompleted"
      role="tabpanel"
      aria-labelledby="inCompleted-tab"
    >
      <slot :data="false"></slot>
    </div>
  </div>
</template>
<script>
export default {
  setup(props, { emit }) {
    const getTab = (val) => {
      emit("getTab", val);
    };
    return {
      getTab,
    };
  },
};
</script>
