<template>
  <div class="container">
    <form
      @submit.prevent="searchNews"
      class="d-flex flex-column justify-content-center"
    >
      <div class="input-group mx-sm-3 mb-2">
        <label class="visually-hidden" for="search">Search</label>
        <input
          type="search"
          name="search"
          v-model="searchTerm"
          id="search"
          class="form-control mb-2 mr-sm-2"
          placeholder="Enter search term here"
        />
        <button class="btn btn-primary mb-2">Search</button>
      </div>
      <p>You are searching for {{ searchTerm }}</p>
    </form>
    <div class="news-list row gx-5">
      <div
        v-bind:key="article.title"
        v-for="article in articles"
        class="col-sm-6 col-md-6 col-lg-3"
      >
        <div class="news-item">
          <img :src="article.urlToImage" />
          <h4 class="article-title">{{ article.title }}</h4>
          <p class="article-body">{{ article.description }}</p>
        </div>
      </div>
    </div>
  </div>
  <ul class="news__list">
    <li
      v-bind:key="article.title"
      v-for="article in articles"
      class="news__item"
    >
      {{ article.title }}
    </li>
  </ul>
</template>
<script>
export default {
  created() {
    let self = this;
    fetch("https://newsapi.org/v2/top-headlines?country=us", {
      headers: {
        Authorization: `Bearer ${import.meta.env.VITE_NEWSAPI_TOKEN}`,
      },
    })
      .then((response) => {
        return response.json();
        //console.log(await response.json())
      })
      .then((data) => {
        self.articles = data.articles;
        console.log(self.articles);
      })
      .catch((err) => {
        console.log(err);
      });
  },

  methods: {
    searchNews() {
      let self = this;
      console.log("Run");
      fetch(
        "https://newsapi.org/v2/everything?q=" +
          self.searchTerm +
          "&language=en",
        {
          headers: {
            Authorization: `Bearer ${import.meta.env.VITE_NEWSAPI_TOKEN}`,
          },
        }
      )
        .then(function (response) {
          return response.json();
        })
        .then(function (data) {
          console.log(data);
          self.articles = data.articles;
        });
    },
  },
  data() {
    return {
      articles: [],
      searchTerm: "",
    };
  },
};
</script>
<style scoped>
.news-item {
  height: 430px;
  margin-top: 15px;
  box-shadow: 0px 0px 10px grey;
  border-radius: 10px;
}

.news-item img {
  border-top-right-radius: 10px;
  border-top-left-radius: 10px;
  width: 100%;
  height: 200px;
  object-fit: cover;
}

.news-item .article-title {
  font-size: 15px;
  padding: 10px;
}

.news-item .article-body {
  font-size: 12px;
  padding: 10px;
}

/*** 
.news-list
{
    display: grid;
    grid-template-columns: repeat(3,33%);
    row-gap: 10px;
    justify-items: center;
    row-gap: 10px;
}**/
</style>