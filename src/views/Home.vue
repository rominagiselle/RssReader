<template>
  <div id="app">
    <form @submit.prevent="getRss">
      <div>
        <label> Ingrese URL de Rss</label>
        <br />
        <input v-model="rssUrl" class="inputSize" style="width:30em" />
      </div>
      <input type="submit" class="send"  />
    </form>
    <div class="items" v-for="item of items" :key="item.title">
      <h3 @click="navigate_to(item)">
        {{ item.title }}</h3>
      <p class="line-clamp"> {{ item.description }}</p>
      <p>{{ item.pubDate }}</p>
    </div>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      rssUrl: "https://www.xatakandroid.com/tag/feeds/rss2.xml",
      items: [],
    };
  },
  methods: {
    async getRss() {
      const res = await fetch(
        `https://api.allorigins.win/get?url=${this.rssUrl}`
      );
      const { contents } = await res.json();
      const feed = new window.DOMParser().parseFromString(contents, "text/xml");
      const items = feed.querySelectorAll("item");
      this.items = [...items].map((el) => ({
        link: el.querySelector("link").innerHTML,
        title: el.querySelector("title").innerHTML,
        author: el.querySelector("author").innerHTML,
        pubDate: el.querySelector("pubDate").innerHTML,
        description: el.querySelector("description").innerHTML
      }));
      this.items.sort(function (a, b) {
        return new Date(b.pubDate) - new Date(a.pubDate);
      })
    },
    async navigate_to(item) {
      this.$router.push({ name: 'rssDetail', params: { id: item } })
    }
  },
};
</script>

<style>
.items {
  text-align: left;
  margin: 1em;
}

.line-clamp {
  display: -webkit-box;
  -webkit-line-clamp: 2;
  -webkit-box-orient: vertical;
  overflow: hidden;
}

label {
  margin: 1em;
  font-weight: bold;
  font-size: large;
}

.send {
  margin: 1em;
  font-size: large;
}
</style>