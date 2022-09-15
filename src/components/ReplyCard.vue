<template>
  <div class="reply-container">
    <div class="vertical-line"></div>
    <article>
      <div class="flex-container">
        <div class="user-likes">
          <a @click="addLike" class="like-btn"
            ><img src="../assets/images/icons/icon-plus.svg" alt="like icon"
          /></a>
          <span class="amount-of-likes">{{ replyScore }}</span>
          <a @click="disLike" class="dislike-btn"
            ><img
              src="../assets/images/icons/icon-minus.svg"
              alt="dislike icon"
          /></a>
        </div>

        <div class="center-content">
          <header>
            <img
              class="user-img"
              :src="require(`../assets/images/avatars/${src}`)"
              alt="user photo"
            />
            <span v-if="name == 'juliusomo'" class="you-tag">you</span>
            <h1 class="user-name">{{ name }}</h1>
            <span class="publishing-time">{{ createdAt }}</span>
          </header>

          <p
            class="user-comment"
            :contenteditable="isEditing"
            @keydown.enter="saveText"
            @blur="saveText"
            :class="{ 'editing-outline': isEditing }"
          >
            <strong class="replyingTo">@{{ replyingTo }}</strong> {{ content }}
          </p>
        </div>

        <a
          v-if="name == 'juliusomo'"
          class="adjust-btn"
          id="delete-btn"
          @click="this.$emit('deleteReply', this.id)"
        >
          <img src="../assets/images/icons/icon-delete.svg" alt="delete icon" />
          Delete
        </a>

        <a v-if="name == 'juliusomo'" class="adjust-btn" @click="editReply">
          <img src="../assets/images/icons/icon-edit.svg" alt="edit icon" />
          Edit
        </a>
      </div>
    </article>
  </div>
</template>

<script>
export default {
  data() {
    return {
      replyScore: null,
      allReplies: null,
      reply: null,
      isEditing: false,
    };
  },
  props: {
    content: String,
    createdAt: String,
    replyingTo: String,
    score: Number,
    name: String,
    src: String,
    id: Number,
  },
  methods: {
    addLike() {
      this.replyScore++;
    },
    disLike() {
      if (this.replyScore == 0) return;
      this.replyScore--;
    },
    editReply() {
      if (this.name !== "juliusomo") return;
      this.isEditing = true;
    },
    saveText() {
      this.isEditing = false;
    },
  },
  mounted() {
    this.replyScore = this.score;
  },
};
</script>

<style lang="scss" scoped>
@import "../assets/scss/main.scss";
</style>
