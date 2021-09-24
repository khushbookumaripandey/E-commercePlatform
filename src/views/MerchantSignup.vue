<template>
  <div class="container">
      <!--    Logo Div-->
    <div class="row">
      <div class="col-12 text-center pt-3">
        <router-link :to="{name : 'Home'}">
          <img id="logo" src="../assets/Cafeshots.png" />
        </router-link>
      </div>
    </div>
    <div class="row">
      <div class="col-12 justify-content-center d-flex flex-row pt-5">
        <div id="signup-div" class="flex-item border">
          <h2 class="pt-4 pl-4">Merchant Create Account</h2>
          <form class="pt-4 pl-4 pr-4">
            <div class="form-group">
              <label>Name</label>
              <input type="firstname" class="form-control" v-model="name" required>
            </div>
            <div class="form-group">
              <label>username</label>
              <input type="username" class="form-control" v-model="username" required>
            </div>
            <div class="form-group">
              <label>Password</label>
              <input type="password" class="form-control" v-model="password" required>
            </div>
            <div class="form-group">
              <label> Confirm Password</label>
              <input type="password" class="form-control" v-model="passwordConfirm" required>
            </div>
            <!-- <div class="form-group">
              <label>Rating</label>
              <input type="rating" class="form-control" v-model="ratings" required>
            </div> -->
            <div class="form-group">
            <label>ImageURL</label>
            <input type="url" class="form-control " v-model="image" required>
          </div>
            <button type="button" @click="merchantSignup" class="btn btn-primary mt-2 py-0">Create Account</button>
          </form>
          <hr>
          <small class="form-text text-dark pt-2 pl-4 text-center">Already Have an Account?</small>
          <p class="text-center"><router-link class="btn btn-dark text-dark text-center mx-auto px-5 py-1 mb-2" :to="{name: 'MerchantSignin'}">SignIn Here</router-link></p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'MerchantSignup',
  props : ["baseURL"],
  data() {
    return {
      name: null,
      username: null,
      password: null,
      ratings:null,
      image:null
    }
  },
  methods : {
    async merchantSignup(e) {
      e.preventDefault();
      if (this.password === this.passwordConfirm) {
        const user = {
          name: this.name,
          username: this.username,
          password: this.password,
          ratings:this.ratings,
          image:this.image
        }

        await axios({
          url : "http://10.177.68.48:8081/merchant/register",
          method : 'post',
          data : user,
          headers: {
            'Content-Type': 'application/json'
          }
        })
        .then(res => {
          this.$router.replace("/");
          swal({
            text: "Merchant signup successful. Please Login",
            icon: "success",
            closeOnClickOutside: false,
          });
        })
        .catch(err => {
          console.log(err);
        });
      } else {
        swal({
          text: "Error! Passwords are not matching",
          icon: "error",
          closeOnClickOutside: false,
        });
      }
    }
  },
  created () {
    console.log('i am created')
  }
}
</script>

<style scoped>

.btn-dark {
  background-color: #e7e9ec;
  color: #000;
  font-size: smaller;
  border-radius: 0;
  border-color: #adb1b8 #a2a6ac #a2a6ac;
}

.btn-primary {
  /* background-color: #f0c14b;
  color: black;
  border-color: #a88734 #9c7e31 #846a29;
  border-radius: 0; */
  border:none;
  outline: none;
  height: 50px;
  width:100%;
  background-color: black;
  color: white;
  border-radius: 4px;
  font-weight: bold;
}
.btn-primary:hover{

  background-color: white;
  border: 1px solid;
  color: black;
        
}

#logo {
  width: 150px;
}

@media only screen and (min-width: 992px) {
  #signup-div {
    width: 40%;
  }
}
.container{
  background-color:rgb(131, 168, 238);
  background-size: cover !important;
  padding: 50px;
  
}

</style>
