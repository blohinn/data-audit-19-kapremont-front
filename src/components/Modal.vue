<template>
  <div class="modal">
    <div class="container-fluid">
      <div class="row">
        <div class="col-12 mt-2">
          <button
            v-on:click="closeModal"
            type="button"
            class="btn btn-link p-0 float-right"
          >
            Закрыть
          </button>
        </div>
        <div class="col-12">
          <p>{{pk}}</p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Modal",
  props: ["pk"],
  methods: {
    closeModal() {
      this.$emit("modalClosed");
    }
  }
};
</script>

<style>
.modal {
  display: block;
  position: fixed;
  top: 0;
  left: 0;
  z-index: 9999;

  height: 100%;
  width: 100%;

  background-color: white;
  opacity: 0.8;
}
</style>
