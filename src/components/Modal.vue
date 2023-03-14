<template>
  <div class="modal-wrap" v-if="modalShow.modal" @click="closeModal">
    <form @submit.prevent class="container modal" @click.stop>
      <div class="form-title">
        <h2>
          {{
            modalShow.action == "update" ? "Update Product" : "Create Product"
          }}
        </h2>
      </div>
      <div class="form-item">
        <label>{{
          modalShow.action == "update" ? "Taxrirlang" : "Title kiriting"
        }}</label>
        <input
          type="text"
          class="form-control mb-4"
          placeholder="Kiriting"
          v-model="postUpdate.title"
        />
      </div>
      <div class="form-item">
        <label>{{
          modalShow.action == "update"
            ? "Update description"
            : "Create description"
        }}</label>
        <textarea
          name=""
          class="form-control mb-4"
          id=""
          rows="5"
          placeholder="..."
          v-model="postUpdate.body"
        ></textarea>
      </div>
      <div class="form-item" v-if="modalShow.action == 'create'">
		<label>UserId</label>
        <input type="number" class="form-control mb-4" v-model="postUpdate.userId" />
      </div>
      <div class="form-btn">
        <button
          class="btn btn-success float-end"
          @click="update(postUpdate.id)"
          v-if="modalShow.action == 'update'"
        >
          Update
        </button>
        <button class="btn btn-success float-center" @click="create()" v-else>Create</button>
      </div>
    </form>
  </div>
</template>
<script>
import axios from "axios";
export default {
  props: {
    modalShow: {
      type: Object,
      require: true,
    },
    postUpdate: {
      type: Object,
      require: true,
    },
  },
  data() {
    return {
    };
  },
  mounted() {
},
methods: {
	update(e) {
		const post = {
        id: e,
        title: this.postUpdate.title,
        body: this.postUpdate.body,
        userId: this.postUpdate.userId,
      };
      this.$emit("updateUser", post);
      axios
        .put(`https://jsonplaceholder.typicode.com/posts/${e}`, post)
        .then((res) => {
        });
		this.modalShow.modal = false;
    },
    create(e) {
      const post = {
        title: this.postUpdate.title,
        body: this.postUpdate.body,
        userId: this.postUpdate.userId,
		show:true
      };
	  this.modalShow.modal = false;
      this.$emit("createUser", post);
      axios
        .post(`https://jsonplaceholder.typicode.com/posts`, post)
        .then((res) => {
          console.log(res);
        });
    },
    closeModal() {
      this.modalShow.modal = false;
    },
  },
};
</script>
<style>
.modal-wrap {
  position: fixed;
  top: 50%;
  left: 50%;
  width: 100%;
  height: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
  transform: translate(-50%, -50%);
  z-index: 3;
  background-color: rgba(0, 0, 0, 0.6);
}

.modal-wrap form {
  position: static;
  padding: 40px ;
  display: block;
  width: 500px;
  height: 600px;
  background-color: #fff;
  border-radius: 20px;
}
</style>
