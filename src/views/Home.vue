<template>
  <div class="home"></div>
</template>

<script>
// @ is an alias to /src
import axios from "axios";
export default {
  data: function() {
    return {
      maxNumber: 0,
      comicsCalificationsNumber: [],
      comic: {}
    };
  },
  name: "Home",
  async mounted() {
    this.$comicsCalificationsNumber = [];
    this.$maxNumber = await this.getMaxNumber().then(res => {
      return res;
    });
    // this.getComicNumber(this.$maxNumber);
    this.getComic(this.$maxNumber);
  },
  methods: {
    getMaxNumber() {
      return new Promise((resolve, reject) => {
        axios
          .get("http://localhost:8080/info.0.json")
          .then(res => {
            if (res) {
              resolve(res.data.num);
            }
          })
          .catch(() => {
            reject(0);
          });
      });
    },
    getComicNumber(maxNumber) {
      let comicNumber = Math.floor(Math.random() * maxNumber);
      do {
        comicNumber = Math.floor(Math.random() * maxNumber);
      } while (this.$comicsCalificationsNumber.includes(comicNumber));
      return comicNumber;
    },
    async getComic(maxNumber) {
      let comicNumber = this.getComicNumber(maxNumber);
      this.$comic = await axios
        .get(`http://localhost:8080/${comicNumber}/info.0.json`)
        .then(res => {
          return res.data;
        });
    }
  }
};
</script>
