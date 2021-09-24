<template>
  <div class="container">
    <div class="row">
      <div class="col-12 text-center">
        <h4 class="pt-3">Your Orders</h4>
      </div>
    </div>
      <div v-if="orders">
    <div  v-for="itr in len" :key="itr" class="row mt-2 pt-3 justify-content-around">
      <div class="col-2"></div>
      <div class="col-md-3 embed-responsive embed-responsive-16by9">
        <img v-bind:src="orderList[itr-1].productImage" class="w-100 card-img-top embed-responsive-item">
      </div>
      <div class="col-md-5 px-3">
        <div class="card-block px-3">
          <h6 class="card-title"><router-link v-bind:to="'/order/'+orderList[itr-1].pid">Order No : {{itr}}</router-link></h6>
          <p class="mb-0">{{orderList[itr-1].totalItems}} item<span v-if="orderList[itr-1].totalItems > 1">s</span></p>
          <p id="item-price" class="mb-0 font-weight-bold">Total Cost : <sup>$</sup>{{totalCost[itr-1]}}</p>
          <p id="item-total-price">Ordered on : {{orderdate[itr-1]}}</p>
        </div>
      </div>
      <div class="col-2"></div>
      <div class="col-12"><hr></div>
    </div>
    </div>
  </div>

</template>

<script>
import axios from 'axios'
 const GET_ORDER = `http://10.177.68.12:8082/project/order/`

export default {

  data() {
    return {
      orders: [],
      len:0,
      // orderList : [],
      totalCost:[],
      // orderdate:[]
    }
  },

  props:["baseURL"],

  name: 'Order',

  methods: {

      listOrders(){
        const self = this
      axios.get(GET_ORDER+localStorage.getItem("uId")).then((response) => {
        console.log(response)
        if(response.status==200){
          self.orders = response.data
          self.len = self.orders.length
          let i
          for(i=0;i<self.len;i++){
              self.totalCost[i] = self.orders[i].totalPrice
              // this.orderdate.push((this.orders[i].createdDate).substring(0,10))
              self.orderList.push({
                productId:self.orders[i].productId,
                productImage: self.orders[i].productImage,
                // totalItems: self.orders[i].orderItems.length,
                price: self.orders[i].price
              })
          }
        }
      },
      (error)=>{
        console.log(error)
      });
    },

  },

  mounted() {
    // this.token = localStorage.getItem("token");
    this.listOrders();
    console.log("i am in orders")
  },
};

</script>

<style scoped>
h4, h5 {
  font-family: 'Roboto', sans-serif;
  color: #484848;
  font-weight: 700;
}

.embed-responsive .card-img-top {
  object-fit: cover;
}
</style>
