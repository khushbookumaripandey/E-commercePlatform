<template>
  <div class="container">
    <div class="row">
      <div class="col-12 text-center">
        <h4 class="pt-3">Our Products</h4>
        <router-link id="add-product" :to="{name : 'AddProduct'}" v-show="$route.name=='MerchantProduct'">
          <button class="btn">Add a new Product</button>
        </router-link>
      </div>
    </div>
    <div class="row">
        <div v-for="product of allProducts" :key="product.id" class="col-md-6 col-xl-4 col-12 pt-3  justify-content-around d-flex">
          <ProductBox :product="product">
          </ProductBox>
        </div>
    </div>
    <!-- <h1 style="text-align: center; color:red; margin:auto;" v-if="!this.product_size">No products found</h1> -->
  </div>
</template>

<script>
import ProductBox from '../../components/Product/ProductBox';
export default {
  name: 'Product',
  components : {ProductBox},
  props : [ "baseURL" , "products" ],
  data () {
    return {
      allProducts: []
    }
  },
  // created() {
  
  // },
  mounted(){
    console.log(' i am product page')
    const skw = this.$route.query.skw || ''
    if (skw === '') {
      this.allProducts = this.products
    } else {
      axios.post('http://10.177.68.48:9000/db/product/search', {"fields":["productName"],"searchTerm": skw})
        .then(res => {
          console.log(res.data)
          this.allProducts = res.data
        })
    }
  }
}
</script>

<style scoped>
h4 {
  font-family: 'Roboto', sans-serif;
  color: #484848;
  font-weight: 700;
}

#add-product {
  float: right;
  font-weight: 500;
}
</style>
