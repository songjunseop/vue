<template>
  <HeaderCont />
  <main id="main">
    <section class="movie__cont">
      <TitleCont name1="THE" name2="Movie" />
      <div class="container">
        <div class="movie__inner">
          <div class="movie__top">
            <div class="title">Top10 콘텐츠</div>
            <!-- <div class="slider">
              <div
                v-for="(movie, index) in topmovies.slice(0, 10)"
                :key="movie.id"
              >
                <span>{{ index + 1 }}</span>
                <img
                  :src="'https://image.tmdb.org/t/p/w154/' + movie.poster_path"
                  :alt="movie.title"
                />
              </div>
            </div> -->
            <swiper
              :slidesPerView="5"
              :spaceBetween="30"
              :autoplay="{
                delay: 1000,
                disableOnInteraction: false,
              }"
              :pagination="{
                clickable: true,
              }"
              :modules="modules"
              class="mySwiper"
            >
              <swiper-slide
                v-for="(movie, index) in topmovies.slice(0, 20)"
                :key="movie.id"
              >
                <span class="num">{{ index + 1 }}</span>
                <img
                  :src="'https://image.tmdb.org/t/p/w154/' + movie.poster_path"
                  :alt="movie.title"
                />
              </swiper-slide>
            </swiper>
          </div>
          <div class="movie__search">
            <form @submit.prevent="SearchMovies()">
              <div>
                <label for="search" class="sr-only">검색하기</label>
                <input
                  type="search"
                  id="search"
                  placeholder="검색하기"
                  v-model="search"
                />
                <button type="submit">검색</button>
              </div>
            </form>
          </div>
          <div class="movie__list">
            <ul>
              <li v-for="movie in movies" :key="movie.id">
                <img
                  :src="'https://image.tmdb.org/t/p/w500/' + movie.poster_path"
                  :alt="movie.title"
                />
                <span class="title">{{ movie.title }}</span>
              </li>
            </ul>
          </div>
        </div>
      </div>
      <ContactCont />
    </section>
  </main>
  <FooterCont />
</template>
<script>
import HeaderCont from "@/components/HeaderCont.vue";
import FooterCont from "@/components/FooterCont.vue";
import TitleCont from "@/components/TitleCont.vue";
import ContactCont from "@/components/ContactCont.vue";
import { Swiper, SwiperSlide } from "swiper/vue";
import { ref } from "vue";
import "swiper/css";
import "swiper/css/pagination";
import { Pagination, Autoplay } from "swiper";
export default {
  components: {
    HeaderCont,
    FooterCont,
    TitleCont,
    ContactCont,
    Swiper,
    SwiperSlide,
  },
  setup() {
    const movies = ref([]);
    const search = ref("avengers");
    const topmovies = ref([]);
    const SearchMovies = () => {
      var requestOptions = {
        method: "GET",
        redirect: "follow",
      };
      fetch(
        // `https://api.themoviedb.org/3/search/movie?api_key=9278d13f704ad0fe53c2263b692efd89&query=${search.value}`,
        `https://api.themoviedb.org/3/search/movie?api_key=8b06fb071e6685e3dacb102957311047&query=${search.value}&type=video&maxResults=28`,
        requestOptions
      )
        .then((response) => response.json())
        .then((data) => {
          movies.value = data.results;
          search.value = "";
          // console.log(movies);
        })
        .catch((error) => console.log("error", error));
    };
    const TopMovies = () => {
      var requestOptions = {
        method: "GET",
        redirect: "follow",
      };
      fetch(
        `https://api.themoviedb.org/3/movie/top_rated?api_key=8b06fb071e6685e3dacb102957311047&language=en-US&page=1`,
        requestOptions
      )
        .then((response) => response.json())
        .then((data) => {
          topmovies.value = data.results;
          console.log(topmovies);
        })
        .catch((error) => console.log("error", error));
    };
    TopMovies();
    SearchMovies();
    return {
      movies,
      search,
      SearchMovies,
      topmovies,
      modules: [Pagination, Autoplay],
    };
  },
};
</script>
<style lang="scss" scoped>
.movie__top {
  margin-bottom: 10%;
  .num {
    font-size: 10vw;
    line-height: 1;
    position: absolute;
    color: #ccc;
    margin-top: -140px;
    z-index: 6;
  }
  .title {
    font-family: var(--subKor_font);
    font-size: 2vw;
  }
}
.swiper-slide {
  padding-top: 130px;
  img {
    position: relative;
    z-index: 10;
  }
}
.movie__search {
  position: relative;
  input {
    border: 2px solid var(--light_border);
    width: 100%;
    background: var(--black);
    border-radius: 50px;
    padding: 1rem 3rem 1rem 2rem;
    color: var(--light_bg);
    font-family: var(--subKor_font);
    margin-bottom: 3%;
  }
  button {
    position: absolute;
    right: 10px;
    top: 9px;
    background: transparent;
    border: 0;
    color: var(--black);
    background: var(--white);
    font-family: var(--subKor_font);
    width: 40px;
    height: 40px;
    border-radius: 50%;
    font-size: 12px;
    transition: opacity 0.3 ease;
    &:active {
      opacity: 0.7;
    }
  }
}
.movie__cont {
  background: var(--dark_bg);
}
.movie__list {
  ul {
    display: flex;
    flex-wrap: wrap;
    justify-content: space-between;

    li {
      flex: 1 1 23%;
      margin: 1%;

      img {
        width: 100%;
        height: 400px;
      }

      p {
        font-family: var(--subKor_font);
        color: var(--white);
        padding-top: 7px;
      }
    }
  }
}

@media (max-width: 800px) {
  .movie__list ul li {
    flex: 1 1 43%;
  }
}
</style>
