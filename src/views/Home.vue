<template>
  <div class="home">
    <div class="container-fluid">
      <div class="row justify-content-center">
        <div class="col-auto">
          <h3>{{ comic.title }}</h3>
        </div>
      </div>
    </div>
    <div class="row justify-content-center">
      <div class="col-auto">
        <img :src="comic.img" class="img-fluid" />
      </div>
    </div>
  </div>
  <!--  -->
  <div v-if="comicRated">
    <div class="row justify-content-center">
      <div class="col-auto">
        <p class="clasfication-title">Thank you for your rating</p>
      </div>
    </div>
    <div class="row justify-content-center">
      <div class="col-auto">
        <button
          type="button"
          class="btn btn-success"
          @click="getComic(maxNumber)"
        >
          Read anothe comic
        </button>
      </div>
    </div>
  </div>
  <div v-if="!comicRated">
    <div class="row justify-content-center">
      <div class="col-auto">
        <p class="clasfication-title">
          Did you have fun reading this comic? Give us your rating!
        </p>
      </div>
    </div>
    <div class="row justify-content-center">
      <div class="col-auto">
        <p class="clasification">
          <input
            id="radio1"
            type="radio"
            name="starts"
            value="5"
            @click="qualify($event.target.value, comic)"
          />
          <label for="radio1">★</label>
          <input
            id="radio2"
            type="radio"
            name="starts"
            value="4"
            @click="qualify($event.target.value, comic)"
          />
          <label for="radio2">★</label>
          <input
            id="radio3"
            type="radio"
            name="starts"
            value="3"
            @click="qualify($event.target.value, comic)"
          />
          <label for="radio3">★</label>
          <input
            id="radio4"
            type="radio"
            name="starts"
            value="2"
            @click="qualify($event.target.value, comic)"
          />
          <label for="radio4">★</label>
          <input
            id="radio5"
            type="radio"
            name="starts"
            value="1"
            @click="qualify($event.target.value, comic)"
          />
          <label for="radio5">★</label>
        </p>
      </div>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import axios from "axios";
export default {
  name: "Home",

  data() {
    return {
      maxNumber: 0,
      comic: {},
      comicRated: false,
      comicsCalifications: [],
      comicsCalificationsNumber: [],
      spinner: false
    };
  },
  async created() {
    this.comicsCalificationsNumber = [];
    this.maxNumber = await this.getMaxNumber().then(res => {
      return res;
    });
    this.getComic(this.maxNumber);
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
      } while (this.comicsCalificationsNumber.includes(comicNumber));
      return comicNumber;
    },
    async getComic(maxNumber) {
      let comicNumber = this.getComicNumber(maxNumber);
      this.comic = await axios
        .get(`http://localhost:8080/${comicNumber}/info.0.json`)
        .then(res => {
          this.comicRated = false;
          return res.data;
        });
    },
    qualify(calification, comic) {
      comic.calification = parseInt(calification);
      this.comicRated = true;
      this.comicsCalifications.push(comic);
      this.comicsCalificationsNumber.push(comic.num);
    }
  }
};
</script>

<style scoped>
.container-fluid {
  margin: 20px 10px;
}
.row {
  margin-bottom: 10px;
}
.clasfication-title {
  font-size: 18px;
  font-weight: bolder;
  margin: inherit;
  text-align: center;
}

#form {
  width: 250px;
  margin: 0 auto;
  height: 50px;
}

#form p {
  text-align: center;
}

#form label {
  font-size: 20px;
}

input[type="radio"] {
  display: none;
}

label {
  color: grey;
  font-size: 40px;
}

.clasification {
  direction: rtl;
  unicode-bidi: bidi-override;
}

label:hover,
label:hover ~ label {
  color: orange;
}

input[type="radio"]:checked ~ label {
  color: orange;
}
</style>
