<template v-on:scroll="onScroll">
  <v-container v-on:scroll="onScroll">
    <v-card flat>
      <v-list dense v-on:scroll="onScroll">
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

    <v-footer app padless>
      <v-col>
        <v-card flat light class="transparent">
          <v-card-text>
            <v-text-field
              hide-details
              clearable
              solo
              :append-outer-icon="'mdi-send'"
              @click:append-outer="postMessage()"
              v-model="newMessage"
            >
            </v-text-field>
          </v-card-text>
        </v-card>
      </v-col>
    </v-footer>

    <v-btn
      fab
      dark
      fixed
      bottom
      right
      color="primary"
      @click="toDown"
      style="margin-bottom: 100px"
      v-show="!onBottom"
    >
      <v-icon>mdi-chevron-down</v-icon>
    </v-btn>
  </v-container>
</template>

<script>
export default {
  data: () => ({
    messages: [],
    newMessage: "",
    onBottom: false,
  }),

  created() {
    fetch("http://localhost:8000/messages")
      .then((response) => response.json())
      .then((data) => (this.messages = data));

    window.addEventListener("scroll", this.onScroll);
  },

  destroyed() {
    window.removeEventListener("scroll", this.onScroll);
  },

  methods: {
    postMessage() {
      const requestOptions = {
        method: "POST",
        body: JSON.stringify({ body: this.newMessage }),
      };

      fetch("http://localhost:8000/messages", requestOptions).then((response) =>
        response.json()
      );

      this.newMessage = "";
    },

    toDown() {
      this.$vuetify.goTo(document.body.scrollHeight);
    },

    onScroll() {
      this.onBottom =
        Math.max(
          window.pageYOffset,
          document.documentElement.scrollTop,
          document.body.scrollTop
        ) +
          window.innerHeight ===
        document.documentElement.offsetHeight;
    },
  },
};
</script>
