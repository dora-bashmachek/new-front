<template>
  <div class="header">
    <div class="content">
    <div class="body">
    <div v-if="fetching" class="loading">Loading...</div>
    <div v-else-if="error" class="error">Oh no... {{ error }}</div>
    <div v-else>
      <div v-if="data">
        <div class="product">
          <div class="title-img">
          <h2 class="title">{{ data.products_by_id.title }}</h2>
          <img class="img" :src="'http://38.242.229.113:8055/assets/' + data.products_by_id.image.id" alt="">
          </div>
          <div class="descr-spec-price">
          <p class="description">{{ data.products_by_id.description }}</p>
          <p class="spec">{{ data.products_by_id.spec }}</p>
          <p class="price">{{ data.products_by_id.price }}$</p>
          </div>
        </div>
      </div>
    </div>
  </div>
</div>
</div>
</template>

<script>
import { useQuery } from "@urql/vue";
import { useRouter, useRoute } from 'vue-router'

export default {
  setup() {
    const router = useRouter();
    const route = useRoute();
    const id = +route.params.id;
    const result = useQuery({
      query: `
        query getProduct($id: ID!) {
          products_by_id(id: $id) {
            id
            title
            price
            spec
            description
            image {
              id
            }
          }
        }
      `, variables: { id }
    });
    return {
      fetching: result.fetching,
      data: result.data,
      error: result.error,
      route
    }
  },
};
</script>

<style scoped>
.header {
  width: 1200px;
  margin: 0 auto;
  background-color: #fff;
  font-family: sans-serif;
}
.content{
  margin: 0 auto;
}
.body{
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin: 0px 20px;
    flex-direction: column;
}
.title{
  font-size: 150%;   
  max-width: 100%;
  margin-bottom: 30px;
  display: block;
  line-height: 21px;
  color: #000;
}
.price{
  font-size: 150%;
  font-weight:600;
  display: flex;
  flex-wrap: wrap;
  align-content: flex-end;
  flex: 1 1 100%;
  max-width: 100%;
}
/* .img {
  width: 125%;
  margin: 15%;
    } */
</style>