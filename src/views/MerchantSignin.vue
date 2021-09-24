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
        <div id="signin-div" class="flex-item border">
          <h2 class="pt-4 pl-4 text-dark">Merchant's Sign-In</h2>
          <form @submit="merchantSignin" class="pt-4 pl-4 pr-4">
            <div class="form-group">
              <label>Merchant's Name</label>
              <input type="name" class="form-control" v-model="name" required>
            </div>
            <div class="form-group">
              <label>Merchant's password</label>
              <input type="password" class="form-control" v-model="password" required>
            </div>
            <small class="form-text text-muted">By continuing, you agree to Cafeshots's Conditions of Use and Privacy Notice.</small>
            <button @click="merchantSignin" class="btn btn-primary mt-2 py-0">
              Continue
              <div v-if="loading" class="spinner-border spinner-border-sm" role="status">
                <span class="sr-only">Loading...</span>
              </div>
            </button>
          </form>
          <hr>
          <small class="form-text text-muted pt-2 pl-4 text-center">New to Cafeshots?</small>
          <p class="text-center"><router-link :to="{name: 'MerchantSignup'}" class="btn btn-dark text-center mx-auto px-5 py-1 mb-2">Create Your Cafeshots Account</router-link></p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'MerchantSignin',
  props : [ "baseURL"],
  data() {
    return {
      name: null,
      password: null,
      
    }
  },
  methods : {
    setMid (id) {
      localStorage.setItem("mId", id);
    },
    async merchantSignin(e) {
      e.preventDefault();
      this.loading = true;

      const user = {
         name: this.name,
        password: this.password
      }

      const self = this
      await axios({
        method: 'post',
        url: "http://10.177.68.48:8081/merchant/login",
        data : JSON.stringify(user),
        headers: {
          'Content-Type': 'application/json'
        }
      })
      .then(res => {
        // localStorage.setItem('token', res.data.token);
        // this.$emit("refreshNav");
        // this.$router.back();
        console.log(res);

        if(res.data){
          console.log("inside response");
          self.setMid(res.data)
          window.location.href = '/merchant'
          // this.$emit("refreshNav");
          // this.$router.push({name : 'Merchant'});
        }
      })
      .catch(err => {
        swal({
          text: "Unable to Log you in!",
          icon: "error",
          closeOnClickOutside: false,
        });
        console.log(err);
      })
      .finally(() => {
        this.loading = false;
      })
    }
  },
  mounted() {
    this.loading = false;
    console.log('i am signup page')
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
  #signin-div {
    width: 50%;

  }
}
.container{
  background-color:rgb(131, 168, 238);
  background-size: cover !important;
  padding: 70px;
  
}
</style>
