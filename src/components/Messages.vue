<template>
  <v-container>
    <v-card flat>
      <v-list dense>
        <v-list-item v-for="message in messages" :key="message.id">
          <v-list-item-content>
            <v-card outlined>
              <v-card-text class="d-flex justify-space-between">
                <span>{{ "#" + message.id }}</span>
                <span>{{ message.timestamp }}</span>
              </v-card-text>
              <v-card-title>{{ message.body }}</v-card-title>
            </v-card>
          </v-list-item-content>
        </v-list-item>
      </v-list>
    </v-card>
  </v-container>
</template>

<script>
export default {
  data: () => ({
    messages: [],
  }),

  created() {
    fetch("http://localhost:8000/messages")
      .then((response) => response.json())
      .then((data) => (this.messages = data));
  },
};
</script>
