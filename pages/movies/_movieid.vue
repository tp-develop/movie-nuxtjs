<template>
  <v-app>
    <div class="container-page">
      <div class="container-loading" v-if="$fetchState.pending">
        <LoadingAction />
      </div>
      <div v-else class="single-movie container">
        <NuxtLink :to="{ name: 'index' }">
          <v-btn color="info"> <v-icon left> mdi-arrow-left </v-icon> </v-btn>
        </NuxtLink>
        <div class="movie-info">
          <div class="movie-img">
            <img
              :src="`https://image.tmdb.org/t/p/w500/${movie.poster_path}`"
              alt=""
            />
          </div>
          <div class="movie-content">
            <h1>Title: {{ movie.title }}</h1>
            <p class="movie-fact tagline">
              <span>Tagline:</span> "{{ movie.tagline }}"
            </p>
            <p class="movie-fact">
              <span>Released:</span>
              {{
                new Date(movie.release_date).toLocaleString('en-us', {
                  month: 'long',
                  day: 'numeric',
                  year: 'numeric',
                })
              }}
            </p>
            <p class="movie-fact">
              <span>Duration:</span> {{ movie.runtime }} minutes
            </p>
            <p class="movie-fact">
              <span>Revenue:</span>
              {{
                movie.revenue.toLocaleString('en-us', {
                  style: 'currency',
                  currency: 'USD',
                })
              }}
            </p>
            <p class="movie-fact">
              <span>Overview:</span> {{ movie.overview }}
            </p>
          </div>
        </div>
      </div>
    </div>
  </v-app>
</template>

<script>
import axios from 'axios'
export default {
  transition: {
    name: 'home',
    mode: 'out-in',
  },
  name: 'single movie',
  data() {
    return {
      movie: 'MOVIE TITLE',
    }
  },
  async fetch() {
    await this.getSingleMovie()
  },
  fetchDelay: 1000,
  methods: {
    async getSingleMovie() {
      const data = axios.get(
        `https://api.themoviedb.org/3/movie/${this.$route.params.movieid}?api_key=6e11619c21e5e8c8b64740255a66bac5`
      )

      const response = await data

      console.log('response : ', response.data)
      this.movie = response.data
    },
  },
  fetchOnServer: false,
}
</script>

<style>
.container-page {
  display: flex;
  flex-direction: column;
  height: 100%;
  width: 100%;
  justify-content: center;
}

.container-loading {
  display: flex;
  justify-content: center;
}
</style>