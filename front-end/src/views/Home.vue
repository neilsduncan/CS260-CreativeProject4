<template>
  <div class="home">
    <section class="image-gallery">
      <div class="image" v-for="item in items" :key="item.id">
        <h2>{{item.title}} CR: {{item.cr}}</h2>

        <div class="horizontal">
          <img :src="item.path" />
          <div class="creatureText">
          <p>-{{item.descript}}</p>
        </div>
        </div>

      </div>
    </section>
  </div>
</template>

<script>
import axios from 'axios';
// @ is an alias to /src

export default {
  name: 'Home',
  data() {
    return {
     items: [],
    }
  },

  created() {
  this.getItems();
},

methods: {
  async getItems() {
    try {
      let response = await axios.get("/api/items");
      this.items = response.data;
      return true;
    } catch (error) {
      console.log(error);
    }
  },
}
}
</script>

<style scoped>

.creatureText{
  padding-left: 16px;
  width: 240px;
}

.horizontal{
  display:inline-flex;
}

.image h2 {
  font-style: italic;
}

/* Masonry */
*,
*:before,
*:after {
  box-sizing: inherit;
}

.image-gallery {
  column-gap: 15.5em;
}

.image {
  margin: 0 0 15.5em;
  display: inline-block;
  width: 100%;
}

.image img {
  width: 100%;
}

/* Masonry on large screens */
@media only screen and (min-width: 1024px) {
  .image-gallery {
    column-count: 4;
  }
}

/* Masonry on medium-sized screens */
@media only screen and (max-width: 1023px) and (min-width: 768px) {
  .image-gallery {
    column-count: 3;
  }
}

/* Masonry on small screens */
@media only screen and (max-width: 767px) and (min-width: 540px) {
  .image-gallery {
    column-count: 2;
  }
}
</style>
