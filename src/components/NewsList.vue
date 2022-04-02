<template>
  <div class="container">
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
  data() {
    return {
      articles: [],
    };
  },
};
</script>
<style scoped>
.news-item {
  height: 430px;
  margin-top: 5px;
  box-shadow: 0px 0px 10px grey;
  margin-top: 10px;
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