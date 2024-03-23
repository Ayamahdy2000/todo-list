<template>
  <div
    class="toast-container position-fixed bottom-0 end-0 p-3"
    :class="failed ? 'toast-error' : 'toast-success'"
  >
    <div
      ref="toaster"
      class="toast"
      role="alert"
      aria-live="assertive"
      aria-atomic="true"
    >
      <div class="toast-body">{{ toasterText }}</div>
    </div>
  </div>
</template>
<script>
import { Toast } from "bootstrap";

export default {
  props: ["toasterText", "failed", "show"],
  data() {
    return { toast: null };
  },
  mounted() {
    this.toast = new Toast(this.$refs.toaster);
     this.$refs.toaster.addEventListener("hidden.bs.toast", () => this.$emit('closed'))
  },
  watch: {
    show() {
      if(this.show){
         this.toast.show();
      }
    },
  },
};
</script>
