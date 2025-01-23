<template>
  <div id="app">
    <h1>Real-Time Chat</h1>
    <div class="chat-box">
      <div v-for="(message, index) in messages" :key="index">
        <div class="message">{{ message }}</div>
      </div>
    </div>
    <input v-model="newMessage" @keyup.enter="sendMessage" placeholder="Type a message..." />
  </div>
</template>

<script>
export default {
  data() {
    return {
      newMessage: '',
      messages: [],
      socket: null
    };
  },
  created() {
    this.socket = new WebSocket('ws://localhost:3000');
    this.socket.onmessage = (event) => {
      this.messages.push(event.data);
    };
  },
  methods: {
    sendMessage() {
      if (this.newMessage.trim()) {
        this.socket.send(this.newMessage);
        this.newMessage = '';
      }
    }
  }
};
</script>

<style scoped>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.chat-box {
  height: 300px;
  overflow-y: scroll;
  border: 1px solid #ccc;
  margin-bottom: 10px;
}

.message {
  background-color: #f1f1f1;
  padding: 10px;
  margin: 5px;
  border-radius: 4px;
}

input {
  padding: 10px;
  width: 80%;
  border: 1px solid #ccc;
  border-radius: 4px;
}
</style>
