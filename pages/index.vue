<template>
  <v-app class="home">
    <Hero @change="onChange" />

    <v-layout row wrap justify-center class="ma-10">
      <LoadingAction v-if="$fetchState.pending" />

      <v-card
        v-else
        class="movie ma-8 pa-5"
        v-for="(movie, index) in movies"
        :key="index"
        @mouseover="onHover(index)"
        @mouseleave="onLeave()"
      >
        <div class="movie-img">
          <v-badge
            bordered
            color="warning"
            overlap
            :content="`${movie.vote_average}`"
          >
            <v-img
              class="image-card"
              contain
              max-height="350"
              max-width="250"
              :src="`https://image.tmdb.org/t/p/w500/${movie.poster_path}`"
            >
              <div class="container-overview pa-5" v-if="currentHover == index">
                <p>{{ movie.overview }}</p>
              </div>
            </v-img>
          </v-badge>
        </div>
        <div class="title-card secondary">
          <p class="title">
            {{ movie.title.slice(0, 25) }}
            <span v-if="movie.title.length > 25">...</span>
          </p>
        </div>
        <div class="release align-center">
          Released:
          {{
            new Date(movie.release_date).toLocaleString('en-us', {
              month: 'long',
              day: 'numeric',
              year: 'numeric',
            })
          }}
        </div>
        <div class="w-100 align-center">
          <v-btn
            class="ma-2 align-center"
            outlined
            color="info"
            :to="{ name: 'movies-movieid', params: { movieid: movie.id } }"
          >
            Get More Info
          </v-btn>
        </div>
      </v-card>
    </v-layout>
  </v-app>
</template>

<script>
import axios from 'axios'

export default {
  head() {
    return {
      title: 'Movie App - Latest Steaming Movie Info',
      meta: [
        {
          hid: 'description',
          name: '',
        },
      ],
      search: '',
    }
  },
  data: () => ({
    allMovie: [],
    movies: [],
    currentHover: null,
  }),
  async fetch() {
    this.getMovie()
  },
  fetchDelay: 1000,
  methods: {
    onLeave() {
      this.currentHover = null
    },
    onHover(index) {
      this.currentHover = index
    },
    onChange(valueInput) {
      console.log(valueInput)

      if (valueInput !== '') {
        this.movies = this.allMovie.filter((element) =>
          element.title.toLowerCase().startsWith(valueInput.toLowerCase())
        )
      } else {
        this.movies = this.allMovie
      }
    },
    async getMovie() {
      const request = axios.get(
        'https://api.themoviedb.org/3/movie/now_playing?api_key=6e11619c21e5e8c8b64740255a66bac5'
      )

      const response = await request
      console.log('getMovie() : ', response.data)

      this.movies = response.data.results
      this.allMovie = response.data.results
      console.log(this.movies)
    },
  },

  fetchOnServer: false,
}
</script>

<style >
v-card {
  text-align: center;
}
.title {
  text-align: center;
}

.align-center {
  text-align: center;
}

.image-card {
  text-align: end;
  align-items: flex-end;
}

.layout {
  display: flex;
  flex-direction: row;
  height: 100%;
  justify-content: center;
}

.container-loading {
  height: 100%;
}

.home {
  display: flex;
  flex-direction: column;
  justify-content: center;
  height: 100%;
}

.container-overview {
  background-color: rgba(0, 0, 0, 0.5);
}

.page-enter-active,
.page-leave-active {
  transition: opacity 0.5s;
}
.page-enter,
.page-leave-to {
  opacity: 0;
}
</style>