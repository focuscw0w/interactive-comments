<template>
  <article>
    <div class="flex-container">
      <div class="user-likes">
        <a @click="addLike" class="like-btn"
          ><img src="../assets/images/icons/icon-plus.svg" alt="like icon"
        /></a>
        <span class="amount-of-likes">{{ score }}</span>
        <a @click="disLike" class="dislike-btn"
          ><img src="../assets/images/icons/icon-minus.svg" alt="dislike icon"
        /></a>
      </div>

      <div class="center-content">
        <header>
          <img
            class="user-img"
            :src="require(`../assets/images/avatars/${src}`)"
            alt="user photo"
          />
          <h1 class="user-name">{{ name }}</h1>
          <span class="publishing-time">{{ createdAt }}</span>
        </header>

        <p class="user-comment">
          {{ content }}
        </p>
      </div>

      <a class="adjust-btn" @click="visibleProfileCard">
        <img src="../assets/images/icons/icon-reply.svg" alt="reply icon" />
        Reply
      </a>
    </div>
  </article>

  <ReplyCard
    v-for="reply in this.allReplies"
    :id="reply.id"
    :content="reply.content"
    :createdAt="reply.createdAt"
    :replyingTo="reply.replyingTo"
    :score="reply.score"
    :name="reply.user.username"
    :src="reply.user.image.png"
    :key="reply.id"
    @deleteReply="deleteReply($event)"
  />

  <ProfileCard v-if="writeComment" @createReply="saveReply($event)" />
</template>

<script>
import ReplyCard from "./ReplyCard.vue";
import ProfileCard from "./ProfileCard.vue";
import json from "../data.json";

export default {
  emits: ["myCommentData", "commentId"],
  components: {
    ReplyCard,
    ProfileCard,
  },
  data() {
    return {
      myJson: json,
      allReplies: [],
      replyByCommentId: [],
      score: null,
      writeComment: false,
    };
  },
  props: {
    id: Number,
    content: String,
    createdAt: String,
    likes: Number,
    name: String,
    src: String,
    reply: Array,
  },
  methods: {
    visibleProfileCard() {
      this.writeComment = !this.writeComment;

      let commentId = this.id;
      commentId--;

      this.replyByCommentId = this.myJson.comments[commentId].replies;
    },
    saveReply(commentData) {
      let myCommentData = [];

      myCommentData = {
        id: commentData.id,
        content: commentData.content,
        createdAt: commentData.createdAt,
        score: 0,
        replyingTo: this.name,
        user: {
          image: {
            png: commentData.image,
          },
          username: commentData.username,
        },
        replies: [],
      };

      this.saveData(myCommentData);
      this.writeComment = false;
    },
    pushReplies(reply) {
      reply.forEach((replyData) => {
        this.allReplies.push(replyData);
      });
    },
    addLike() {
      this.score++;
    },
    disLike() {
      if (this.score == 0) return;
      this.score--;
    },
    deleteReply(replyId) {
      this.allReplies = this.allReplies.filter((reply) => reply.id !== replyId);
    },
    saveData(commentData) {
      this.replyByCommentId.push(commentData);
      this.allReplies.push(commentData);
    },
  },
  mounted() {
    this.pushReplies(this.reply);
    this.score = this.likes;
  },
};
</script>

<style lang="scss" scoped>
@import "../assets/scss/main.scss";
</style>
