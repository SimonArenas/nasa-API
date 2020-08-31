<template>
  <v-app>
    <!-- Header -->
    <v-card class="overflow-hidden">
      <v-app-bar
        height="200px"
        color="#6A76AB"
        dense
        dark
        src="./assets/bg.jpg"
        fade-img-on-scroll
      >
        <template v-slot:img="{ props }">
          <v-img
            v-bind="props"
            gradient="to top right, rgba(100,115,201,.7), rgba(25,32,72,.7)"
          ></v-img>
        </template>
        <v-toolbar-title
          ><h2>
            Pick a date and discover a photo of the space from that day 🌙
          </h2></v-toolbar-title
        >
      </v-app-bar>
    </v-card>

    <!-- Data picker -->
    <v-row class="mt-10" justify="center">
      <v-date-picker
        v-model="picker"
        color="indigo darken-3"
        
      ></v-date-picker>
      <v-col cols="12">
        <v-btn class="white--text" color="indigo lighten-1" @click="send()"
          >Let's go</v-btn
        >
      </v-col>
    </v-row>

    <!-- API response -->
    <div v-if="data">
      <v-col cols="12">
        <h1 class="date-title deep-purple lighten-3">
          <h3>{{ data.title }}</h3>
        </h1>
      </v-col>
      <v-col cols="12">
        <div class="img">
          <v-img :src="data.url" aspect-ratio="2" contain></v-img>
        </div>
      </v-col>
      <v-col cols="12">
        {{ data.explanation }}
      </v-col>
    </div>
  </v-app>
</template>

<script>
import axios from "axios";
export default {
  data() {
    return {
      data: null,
      picker: new Date().toISOString().substr(0, 10),
    };
  },

  methods: {
    getData() {
      axios
        .get(
          `https://api.nasa.gov/planetary/apod?api_key=t5aaZf3pnlEefCZbnYioofFJ9F1XfwdSozOkiGE5&date=${this.picker}`
        )
        .then((response) => {
          this.data = response.data;
        })
        .catch((e) => console.log(e));
    },
    send() {
      this.getData();
    },
  },
};
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;

  text-align: center;
}
.img {
  img {
    text-align: center;
  }
}
.title {
  h1 {
    margin-top: 20px;
    padding-bottom: 20px;
    font-size: 30px;
  }
}
.date-title {
  h3 {
    color: white;
  }
}
</style>
