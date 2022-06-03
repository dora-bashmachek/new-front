<template>
  <div class="header">
    <div class="content">
    <div class="body">
      <div class="shapka">
        <p class="prod">Products</p>
      </div>
      <div class="search d4">
     <form>    
       <input  type="text" v-model="search" placeholder="search"/>
         <button type="submit"><font-awesome-icon icon="fa-solid fa-magnifying-glass" /></button>
         </form>   
    </div>
    <div v-if="fetching" class="loading">Loading...</div>
    <div v-else-if="error" class="error">Oh no... {{ error }}</div>
    <div v-else>
      <div v-if="data" class="products">
        <div v-for="p in data.products" :key="p.id" @click="move(p.id)" class="product_card">
          <img class="img" :src="'http://38.242.229.113:8055/assets/' + p.image.id + '?width=190&height=200'" alt="">
          <div class="title-price">
          <p class="title">{{ p.title }}</p>
          <p class="price">{{ p.price }}$</p>
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
import { ref } from "@vue/reactivity";
import { useRouter, useRoute } from 'vue-router'

export default {
  setup() {
    const search = ref(null);
    const router = useRouter()
    const route = useRoute()
    const result = useQuery(
      {
        query: `
        query($search: String) {
          products(search: $search) {
            id
            title
            price
            spec
            image {
              id
            }
          }
        }
      `, variables: { search }
      },
    );
    function searchProducts() {
      result.executeQuery()
    }

    function move(id) {
      router.push("/products/" + id)
    }

    return {
      search,
      fetching: result.fetching,
      data: result.data,
      error: result.error,
      searchProducts,
      move
    };
  },
};
</script>

<style scoped>
/* .d4 {background: #F15B42;} */
.d4 form {
  background: #fffffe;
  border-bottom: 4px solid #d9dcd9;
}
.d4 input, .d4 button {
  /* border: none; */
  outline: none;
  background: transparent;
}
.d4 input {
  width: 100%;
  height: 42px;
  padding-left: 15px;
}
.d4 button {
  height: 42px;
  width: 42px;
  position: absolute;
    top: 444px;
    right: 180px;
  cursor: pointer;
}
.d4 button:before {
  content: "\f178";
  font-family: FontAwesome;
  font-size: 20px;
  color: #be290e;
}
.shapka{
  background-image: url("https://ltdfoto.ru/images/2022/06/03/SNIMOK-EKRANA-25cd023ecaf1f94ac.png");
    height: 380px;
    width: 980px;
    background-size: contain;
    background-repeat: no-repeat;
}
.prod{display: flex;
    flex-direction: row-reverse;
    padding: 11%;
    font-size: 450%;
}
.header {
  width: 1200px;
  margin: 0 auto;
  background-color: #fff;
  font-family: sans-serif;
}
.content{
  margin: 0 auto;
  /* background-color: #0400ff; */
}
.body{
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin: 0px 20px;
    flex-direction: column;
}
.search {
    display: flex;
    flex-direction: row;
    flex-wrap: nowrap;
    justify-content: flex-end;
    width: 74%;
    margin: 3%;
}
.products {
    display: flex;
    width: 100%;
    text-align: center;
    flex-wrap: wrap;
    justify-content: flex-start;
}
.product_card{
  width: 23%;
  padding: 3%;
  margin: 1%;
  border: #00000059 solid 1px;
  border-radius: 5%;
      box-shadow: inset 0 0 10px 5px rgb(214 216 211);
  text-align: left;
}
.product_card:hover {
  border: #0400ff solid 1px;
  transition: 0.5s;
  cursor: pointer;
 
}
.img{
    margin: 7%;
    padding: 5%;
}
.title{
  font-size: 100%;    
  /* flex: 1 1 100%; */
  max-width: 100%;
  margin-bottom: 30px;
  display: block;
    /* font-size: 18px; */
    line-height: 21px;
    /* margin-bottom: 5px; */
    color: #000;
    /* text-decoration: none; */
    /* transition: color .3s linear; */
    /* word-break: break-word; */
}
.price{
  font-size: 100%;
  font-weight:600;
  display: flex;
  flex-wrap: wrap;
  align-content: flex-end;
  flex: 1 1 100%;
  max-width: 100%;
}
.title-price {
  display: flex;
  flex-wrap: wrap;
  flex: auto;
  max-width: 100%
}
</style>