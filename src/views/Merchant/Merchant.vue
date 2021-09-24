<template>
  <div class="merchantPanel container">
      <!-- <router-link :to="{name : 'MerchantProduct'}">
        <button type="button" class="btn btn-primary btn-lg">Merchant Products</button>
      </router-link> -->
      <nav class="m-nav">
        <h3>MERCHANT PRODUCT</h3>
        <button @click="openForAddNew" class="btn btn-success"> ADD +</button>
      </nav>
      <div class="product-container">
        <div class="product-card" v-for="(product, productId) in allProducts" :key="product.id">
           <img :src="product.image" style="height:250px;">
          <h4>
            {{product.productName}}
          </h4>
          <p>Price: {{product.productPrice}}</p>
           <p>Quantity: {{product.availble}}</p>
          <p>Sold: {{product.sold}}</p>
          <p>Ratings: {{product.ratings}}</p>
          
           <!-- <p>Quantity: {{product.productQuantity}}</p> -->
          <!-- <p>Ratings: {{product.ratings}}</p> -->
          <!-- <p>Ratings: {{product.ratings}}</p> -->
          <div class="action">
            <button class="btn btn-outline-danger btn-sm mr-2" @click="deleteProduct(productId)">DELETE</button>
            <button class="btn btn-outline-primary btn-sm" @click="openEditModal(product, productId)">EDIT</button>
          </div>
        </div>
      </div>
      <div v-if="openModal" class="modal">
          <div class="product-edit">
            <h3>Update/Add product</h3>
            <div class="mb-3">
              <label class="form-label">ProductName</label>
              <input class="form-control" v-model="product.productName" >
            </div>
            <div class="mb-3">
              <label class="form-label">ProductQuantity</label>
              <input class="form-control" v-model="product.productQuantity">
            </div>
            <div class="mb-3">
              <label class="form-label">ProductPrice</label>
              <input class="form-control" v-model="product.productPrice" >
            </div>
            <div class="mb-3">
              <label class="form-label">ProductImage</label>
              <input class="form-control" v-model="product.productImage">
            </div>
            <div class="mb-3">
              <label class="form-label">ProductCategory</label>
              <input class="form-control" v-model="product.productCategory">
            </div>
            <div class="mb-3">
              <label class="form-label">Availble</label>
              <input class="form-control" v-model="product.availble">
            </div>
            <div class="mb-3">
              <label class="form-label">Sold</label>
              <input class="form-control" v-model="product.sold">
            </div>
            <!-- <div class="mb-3">
              <label class="form-label">Ratings</label>
              <input class="form-control" v-model="product.ratings">
            </div> -->
            <div class="action">
              <button class="btn btn-outline-danger btn-sm mr-2" @click="closeModal">Close</button>
              <button class="btn btn-outline-primary btn-sm" @click="saveAction">Save</button>
            </div>
          </div>
      </div>
  </div>
</template>

<script>
import axios from 'axios'
const GET_ALL_PRODUCT_URL = `http://10.177.68.48:8081/merchant/${localStorage.getItem('mId')}`
const CREATE_NEW_PRODUCT = `http://10.177.68.48:8081/merchant/addProduct/${localStorage.getItem('mId')}`
const UPDATE_PRODUCT = (pid) =>`http://10.177.68.48:8081/merchant/updateProduct/${localStorage.getItem('mId')}/${pid}`
const DELETE_PRODUCT= (pid) => `http://10.177.68.48:8081/merchant/deleteProducts/${localStorage.getItem('mId')}/${pid}`


export default {
  data () {
    return {
      allProducts: {},
      openModal: false,
      product: {
        productName: "",
        productQuantity: 0,
        productPrice: 0,
        productImage: "",
        productCategory: "",
        availble: 0,
        sold: 0,
        ratings: 0,
      },
      editProductId: null
    }
  },
  methods: {
    closeModal () {
      this.openModal = false
    },
    openForAddNew () {
      this.openModal = true
    },
    clearProduct () {
      this.product.productName = ''
      this.product.productQuantity = 0
      this.product.productPrice = 0
      this.product.productImage = ''
      this.product.productCategory = ''
      this.product.availble = 0
      this.product.sold = 0
      this.product.ratings = 0
      this.openModal = false
      this.editProductId = null
      this.getAllProduct()
    },
    openEditModal (product, productId) {
      this.product.productName = product.productName || ''
      this.product.productQuantity = product.productQuantity || 0
      this.product.productPrice = product.productPrice || 0
      this.product.productImage = product.productImage || ''
      this.product.productCategory = product.productCategory || ''
      this.product.availble = product.availble || 0
      this.product.sold = product.sold || 0
      this.product.ratings = product.ratings || 0
      this.openModal = true
      this.update = true
      this.editProductId = productId || null
    },
    saveAction () {
      if (this.update) {
        this.updateProduct(this.product, this.editProductId)
      } else {
        this.saveProduct()
      }
    },
    
    // Delete Start
   successFunction () {
    //  Swal.fire('Successfully Deleted product!')
      console.log('product deleted successfully')
      this.clearProduct()
      // console.log(localStorage.getItem('merchantId'))
   },
    failFunctionDel () {
      // Swal.fire('Unabled to Delete product!')
      console.log('product failed to delete')
    },
    deleteProduct (productId) {
     axios.delete(DELETE_PRODUCT(productId))
        .then(this.successFunction)
        .catch(this.failFunctionDel)
    },
 // end

    // update product start
    onSuccessUpdateProduct (res) {
      this.clearProduct()
      // Swal.fire('Successfully Updated  product!')
      console.log('product edited successfully')
    },
    onFailUpdateProduct (res) {
      // Swal.fire('Unabled to Update product')
      console.log('failed to update product')
    },
    updateProduct (product, productId) {
      axios.put(UPDATE_PRODUCT(productId),product)
        .then(this.onSuccessUpdateProduct)
        .catch(this.onFailUpdateProduct)
    },
    // update product end

    // save new product start
    onSuccessSaveProduct (res) {
      // Swal.fire('Successfully Saved product!!')
      this.clearProduct()
      
      console.log('product saved successfully')
    },
    onFailSaveProduct (res) {
      //  Swal.fire('Failed to Saved product!!')
      console.log('failed to save product')
    },
    saveProduct () {
      axios.post(CREATE_NEW_PRODUCT, this.product)
        .then(this.onSuccessSaveProduct)
        .catch(this.onFailSaveProduct)
    },
    // end

    // this if get all Product start
    onSuccessGetAllProduct (res) {
      console.log(res.data.productsMap);
      this.allProducts = res.data.productsMap;
      //  this.getAllProduct()
      console.log('getting all products')
    },
    onFailGetAllProduct (res) {
      console.log(res)
      console.log('failed to get all products')

    },
    getAllProduct () {
      axios.get(GET_ALL_PRODUCT_URL)
        .then(this.onSuccessGetAllProduct)
        .catch(this.onFailGetAllProduct)
    }
    // end
  },
  mounted() {
    console.log('i am merchant')
    this.getAllProduct()
  }
}
</script>

<style scoped>
/* .merchantPanel{
  display : flex;
  flex-direction: column;
  align-items: center;
}
.merchantPanel button{
  font-size: 30px;
  width: fit-content;
  margin: 55px 0;
} */
.m-nav {
  display: flex;
  justify-content: space-between;
  margin: 20px 0;
}
.product-container {
  display: flex;
  flex-wrap: wrap;
}
.product-card {
  width: 400px;
  margin: 15px;
  padding: 15px;
  box-shadow: 2px 2px 4px #777;
  border-radius: 8px;
  /* height: 100px; */
}

.modal {
  position: fixed;
  height: 100vh;
  width: 100vw;
  top: 0;
  left: 0;
  display: flex;
  justify-content: center;
  align-items: center;
  background: rgba(0,0,0, 0.3);
  z-index: 10;
}
.product-edit {
  width: 500px;
  max-height: 80vh;
  overflow: auto;
  background: white;
  border-radius: 8px;
  padding: 15px;
}
</style>