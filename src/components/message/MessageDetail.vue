<template>
  <div class="message-detail" v-if="selectedMenu === 'detail' && selectedMessage">
    <h2>쪽지 상세보기</h2>
    <div><strong>작성자:</strong> {{ selectedMessage.senderNickname || 'N/A' }}</div>
    <div><strong>수신자:</strong> {{ selectedMessage.receiverNickname || 'N/A' }}</div>
    <div><strong>전송 시간:</strong> {{ selectedMessage.sentAt }}</div>
    <hr>
    <h3>{{ selectedMessage.title }}</h3>
    <p>{{ selectedMessage.content }}</p>
  </div>
</template>

<script>
import { apiClient } from "@/index"

export default {
  data() {
    return {
      selectedMessage: null,
      selectAll: false,
    };
  },
  mounted() {
  },
  methods: {
    toggleSelectAll() {
      this.selectAll = !this.selectAll;
    },
    selectMessage(message) {
      this.selectedMessage = message;
      this.selectedMenu = "detail";
    },
    deleteSelectedMessages() {
      this.sentMessages = this.sentMessages.filter(
        (message) => !message.selected
      );
    },
  },
};
</script>
<style scoped lang="css">

</style>