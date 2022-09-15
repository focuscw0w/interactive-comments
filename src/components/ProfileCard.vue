<template>
  <article id="profile">
    <div class="flex-container">
      <img
        class="user-img"
        src="../assets/images/avatars/image-juliusomo.png"
        alt=""
      />

      <form action="#">
        <textarea
          v-model="myComment"
          name="profile-comment"
          id="profile-comment"
          cols="75"
          rows="4"
          placeholder="Add a comment..."
        ></textarea>
      </form>

      <button @click="sendComment" type="submit" id="send-comment">Send</button>
    </div>
  </article>
</template>

<script>
import json from "../data.json";

export default {
  data() {
    return {
      myJson: json,
      myComment: "",
      currentUser: json.currentUser,
      username: json.currentUser.username,
      image: json.currentUser.image.png,
      createdAt: "now",
      mark: "you",
    };
  },
  methods: {
    sendComment() {
      const data = this.myJson.comments;
      const replies = data.map((comment) => comment.replies);

      replies.forEach((reply) => {
        this.replyIds = reply.map((reply) => reply.id);
      });

      this.$emit("createReply", {
        content: this.myComment,
        username: this.username,
        image: this.image,
        createdAt: this.createdAt,
        mark: this.mark,
        id: Math.max(...this.replyIds) + 1,
      });
    },
  },
};
</script>

<style lang="scss" scoped>
@import "../assets/scss/main.scss";
</style>
