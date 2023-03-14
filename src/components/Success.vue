<template>
  <div class="modal-wrap" v-if="success.show" @click="success.show = false">
    <div class="modal_success" @click.stop>
      <h1>
        {{
          success.action == "create"
            ? "Product Creatred Successfully!!": (success.action == "err" ?"Xatolik yuz berdi":"You wan't be able to revert this")
            
        }}
      </h1>
      <h1 v-if="success.action == 'delete'">
        You won't be able to revert this
      </h1>
      <button
        class="btn float-center mt-5"
        :class="success.action == 'create' ? 'btn-success' : 'btn-danger'"
        @click="clickMethod()"
      >
        {{ success.action == "create" ? "OK" : "Yes, delete it" }}
      </button>
      <button
        class="btn btn-success mt-5"
        v-if="success.action == 'delete'"
        @click="success.show = false"
      >
        Cancel
      </button>
    </div>
  </div>
</template>
<script>
export default {
  props: {
    success: {
      tuype: Object,
      require: true,
    },
  },
  data() {
    return {
      agree: false,
    };
  },
  methods: {
    clickMethod() {
      console.log("dkery x2");
      this.success.show = false;
      this.success.agree = true;
      this.$emit("agree", this.success.agree);
    },
  },
};
</script>
<style>
.modal_success {
  text-align: center;
  position: static;
  padding: 50px;
  display: block;
  width: 500px;
  background-color: #ffffff;
  border-radius: 20px;
}
</style>
