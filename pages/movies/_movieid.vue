<template>
    <Loading v-if="$fetchState.pending" />
    <div v-else class="container single-movie">
        <Nuxt-Link class="button" :to="{name:'index'}">Back</Nuxt-Link>
        <div class="movie-info">
            <div class="movie-img">
                <img :src="`https://image.tmdb.org/t/p/w500/${movie.poster_path}`" alt="">
            </div>
            <div class="movie-content">
                <h1>Title: {{ movie.title }}</h1>
                <p class="movie-fact tagline">
                    <span>Tagline:</span>"{{ movie.tagline}}"
                </p>
                <p class="movie-fact">
                    <span>Released:</span>
                    {{
                        new Date(movie.release_date).toLocaleString('en-us',{
                            month:'long',
                            day:'numeric',
                            year:'numeric',
                        })
                    }}
                </p>
                <p class="movie-fact">
                    <span>Duration: <span>{{movie.runtime}} minutes</span></span>
                </p>
                <p class="movie-fact">
                    <span>Revenue:</span>
                    {{ movie.revenue.toLocaleString('en-us',{
                        style:'currency',
                        currency:'USD'
                    })}}
                </p>
                <p class="movie-fact">
                    <span>Overview:</span>

                    {{ movie.overview}}
                </p>
            </div>
        </div>
        <h1>{{ this.movie.title }}</h1>
    </div>
</template>

<script>
    import axios from 'axios'
    export default {
        head(){
            return{
                title:this.movie.title
            }
        },
        name:'single-movie',
        data(){
            return{
                movie:null
            }
        },
        async fetch(){
            await this.getSingleMovie()
        },
        fetchDelay:1000,
        methods:{
            async getSingleMovie(){
                const data =  axios.get(
                `https://api.themoviedb.org/3/movie/${this.$route.params.movieid}?api_key=d0fbd4fd92ad276b3be48ccc76175b51`
                )
                const result = await data
                this.movie = result.data
            }
        }
    }
</script>

<style scoped>
    .container {
        max-width: 1400px;
        margin: 0 auto;
    }
    .single-movie {
        color: #fff;
        min-height: 100vh;
        display: flex;
        flex-direction: column;
        justify-content: center;
        padding: 32px 16px;
    }
    .button {
        align-self: flex-start;
        margin-bottom: 32px;
    }
    .movie-info {
        display: flex;
        flex-direction: column;
        align-items: center;
        gap: 32px;
        color: #fff;
    }
    .movie-img img {
        max-height: 500px;
        width: 100%;
    }
    h1 {
        font-size: 56px;
        font-weight: 400;
    }
    .movie-fact {
        margin-top: 12px;
        font-size: 20px;
        line-height: 1.5;
    }
    .movie-fact span {
        font-weight: 600;
        text-decoration: underline;
    }
    .tagline {
        font-style: italic;
    }
    .tagline span {
        font-style: normal;
    }

    @media (min-width: 800px) {
        .movie-info {
            flex-direction: row;
            align-items: flex-start;
        }
    }
    @media (min-width: 800px) {
        .movie-img img {
            max-height: 700px;
            width: initial;
        }
    }
</style>