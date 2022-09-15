<template>
  <div class="container">
    <CommentCard
      v-for="profile in this.myJson.comments"
      :id="profile.id"
      :content="profile.content"
      :createdAt="profile.createdAt"
      :likes="profile.score"
      :name="profile.user.username"
      :src="profile.user.image.png"
      :reply="profile.replies"
      :key="profile.id"
      @commentId="this.commentId = $event"
    />
  </div>
</template>

<script>
import json from "./data.json";
import CommentCard from "./components/CommentCard.vue";

export default {
  name: "App",
  components: {
    CommentCard
  },
  data() {
    return {
      myJson: json
    }
  }
}
</script>

<style lang="scss">
@import "./assets/scss/main.scss";
</style>
