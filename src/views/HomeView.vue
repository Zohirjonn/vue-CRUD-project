<template>
  <div class="header">
    <div class="container mb-5">
      <button class="btn btn-primary mt-5 float-end" @click="createUser">
        Create User
      </button>
      <table class="table table-striped table-hover">
        <thead>
          <tr>
            <th scope="col">ID</th>
            <th scope="col">Title</th>
            <th scope="col">Body</th>
            <th scope="col">userId</th>
            <th scope="col" class="col-2">Action</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="post in posts" :key="post.id">
            <th scope="row">{{ post.id }}</th>
            <td>{{ post.title }}</td>
            <td>{{ post.body }}</td>
            <td>{{ post.userId }}</td>
            <td>
              <button class="btn btn-success" @click="update(post.id, $event)">
                Update
              </button>
              <button class="btn btn-danger" @click="deleteMethod(post.id)">
                DELETE
              </button>
            </td>
          </tr>
        </tbody>
      </table>
      <div class="col-12 d-block">
        <button @click="getdata" class="btn btn-success">Get data</button>
      </div>
    </div>
    <Modal
      :modalShow="modalShow"
      :posts="posts"
      :postUpdate="postUpdate"
      class="modalshow"
      @updateUser="updateUser"
      @createUser="newUser"
    />
    <Success :success="success" @agree="agreeMethod" />
  </div>
</template>

<script>
import axios from "axios";
import Modal from "../components/Modal.vue";
import Success from "../components/Success.vue";
export default {
  components: {
    Modal,
    Success,
  },
  data() {
    return {
      posts: [],
      modalShow: {
        modal: false,
        action: "",
        updateId: null,
      },
      postUpdate: {
        id: null,
        title: "",
        body: "",
        userId: null,
      },
      success: {
        show: false,
        action: "",
        agree: false,
      },
      deleteID: null,
    };
  },
  mounted() {},
  methods: {
    async getdata() {
      const responce = await axios.get(
        "https://jsonplaceholder.typicode.com/posts?userId=1"
      );
      const res = await responce.data;
      this.posts = res;
    },
    agreeMethod(item) {
      this.success.agree = item;
      if (item && this.deleteID !== null) {
        this.posts = this.posts.filter((t) => t.id !== this.deleteID);
      }
    },
    createUser() {
      this.modalShow.modal = true;
      this.modalShow.action = "create";
      this.postUpdate.id = null;
      this.postUpdate.title = "";
      this.postUpdate.body = "";
      this.postUpdate.userId = null;
    },
    update(id) {
      this.modalShow.modal = true;
      this.modalShow.action = "update";
      this.updateId = id;
      this.posts.forEach((t) => {
        if (t.id == id) {
          this.postUpdate.id = id;
          this.postUpdate.title = t.title;
          this.postUpdate.body = t.body;
          this.postUpdate.userId = t.userId;
        }
      });
    },
    updateUser(item) {
      this.posts.forEach((t) => {
        if (t.id === item.id) {
          t.id = item.id;
          t.title = item.title;
          t.body = item.body;
          t.userId = item.userId;
        }
      });
    },
    newUser(item) {
      if (item.title !== "" && item.body !== "") {
        item.id = this.posts.length + 1;
        this.posts.push(item);
      } else {
        this.success.action = "err";
      }
      this.success.show = item.show;
      this.success.action = "create";
    },
    deleteMethod(id) {
      this.success.show = true;
      this.success.action = "delete";
      this.deleteID = id;
      this.success.agree = false;
    },
  },
};
</script>
<style>
.btn {
  margin: 0 5px;
}
</style>
