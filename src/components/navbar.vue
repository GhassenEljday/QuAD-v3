<template>
  <nav id="navbar" class="navbar navbar-expand-lg navbar-light" :class="navbar">
    <router-link to="/" replace><a id="logo" class="nav-link" 
            >QuAD <span class="sr-only">(current)</span></a
          ></router-link>
          
    <button
      class="navbar-toggler"
      type="button"
      data-toggle="Collapse"
      data-target="#navbarTogglerDemo02"
      aria-controls="navbarTogglerDemo02"
      aria-expanded="false"
      aria-label="Toggle navigation"
    >
      <span class="navbar-toggler-icon"></span>
    </button>

    <div class="collapse navbar-collapse" id="navbarTogglerDemo02">
      <ul class="navbar-nav mr-auto mt-2 mt-lg-0">
        <li class="nav-item active">
          <router-link to="/" replace><a id="about" class="nav-link" 
            >Home <span class="sr-only">(current)</span></a
          ></router-link>
          
        </li>
        <li class="nav-item active">
          <router-link to="/about" replace>
          <a id="about" class="nav-link"
            >About us <span class="sr-only">(current)</span></a
          >
          </router-link>
        </li>
        <li class="nav-item active">
          <router-link to="/contact" replace>
          <a id="about" class="nav-link"
            >contact <span class="sr-only">(current)</span></a
          >
          </router-link>
        </li>
      </ul>
      <form v-on:submit.prevent="companySide" v-if="freeLancerLogin" class="form-inline my-2 my-lg-0">
        <input
          class="form-control mr-sm-2"
          type="email"
          placeholder="freelancer Email"
          v-model="Email"
        />
        <input
          class="form-control mr-sm-2"
          type="password"
          placeholder="Password"
          v-model="Password"
        />
        <!-- this button gonna switch beetween freelancer login and company login -->
        <button
          id="btn"
          class="btn btn-primary"
          type="submit"
          data-toggle="collapse"
        >
          company
        </button>
        <button
          id="btn"
          class="btn btn-primary"
          type="submit"
          data-toggle="collapse"
          @click="this.login"
        >
          Log in
        </button>
      </form>
      <form v-on:submit.prevent="freelancerSide" v-if="companyLogin" class="form-inline my-2 my-lg-0">
        <input
          class="form-control mr-sm-2"
          type="email"
          placeholder="company Email"
          v-model="Email"
        />
        <input
          class="form-control mr-sm-2"
          type="password"
          placeholder="Password"
          v-model="Password"
        />
        <!-- this button gonna switch beetween freelancer login and company login -->
        <button
          id="btn"
          class="btn btn-primary"
          type="submit"
          data-toggle="collapse"
        >
          Freelancer
        </button>
        <button
          id="btn"
          class="btn btn-primary"
          type="submit"
          data-toggle="collapse"
          @click="this.login"
        >
          Log in
        </button>
      </form>
    </div>
  </nav>
</template>
<script>
import axios from "axios";
export default {
name: "navbar",
  data() {
    return {
      freeLancerLogin: true,
      companyLogin: false,
      Email:"",
      Password:""
    };
  },
  
    methods: {
    freelancerSide() {
      console.log("freelancerSide");
      this.freeLancerLogin = true;
      this.companyLogin = false;
    },
    companySide() {
      console.log("companyLogin");
      this.freeLancerLogin = false;
      this.companyLogin = true;
    },
    login(){
      if(this.freeLancerLogin){
       axios({
        method: "post",
        url: "http://localhost:3008/login",
        data: {
          email:this.Email,
          password:this.Password
        }
       }).then(data=>{
         if(data.data.login){
           alert("check again")
         }else{
           this.$emit('childToParent',data.data)
           console.log(data)
          //  this.$router.push({path:`/freelancer/${data.data.id}/profile`})
         }
       })
      }else{
axios({
        method: "post",
        url: "http://localhost:3008/login/company",
        data: {
         email:this.Email,password:this.Password
        }
       }).then(data=>{
         if(data.data.login){
           alert("check again")
         }else{ 
           console.log(data)
           this.$emit('companydata',data.data)
          //  this.$router.push({path:`/company/${data.data.id}/profile`})
         }
       })
      }
    }
  }
};

</script>
<style scoped>
#about {
  color: white;
  font-size: 20px;
}

#logo {
  color: white;
  font-size: 34px;
}
#btn {
  margin: 5px;
  text-decoration: none;
}
</style>
