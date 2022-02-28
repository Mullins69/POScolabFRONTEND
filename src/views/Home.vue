<template>
  <section class="home">
      <div class="container">
          <div class="row">
              <div class="col">
                  <div class="header">
            <h1>M&M POINT OF SALE SYSTEM</h1>
            <h2>Register Now</h2>
            <button type="button" class="btn btn-outline-primary"><router-link class="router" :to="{ name: 'Register' }"> Register
                                    </router-link></button>
            <h2>Already a member?</h2>
            <button type="button" class="btn btn-outline-primary"><router-link class="router"  :to="{ name: 'Login' }">Login</router-link></button>
            <h2>Already Logged in?</h2>
            <button type="button" class="btn btn-outline-primary"><router-link class="router" :to="{ name: 'Products' }">Click Here</router-link></button>
        </div>  
              </div>
          </div>
      </div>
  </section>
</template>

<script>
export default {
    data(){
        shownav: false
    }
}
</script>

<style scoped>
.home{
  background-color: #212529;
   display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
  min-height: 100vh;
}
.router{
    text-decoration: none;
}
.router:hover{
    color: white;
}
.btn-outline-primary {
    
    border-color: #0DB8DE;
    color: #0DB8DE;
    border-radius: 0px;
    font-weight: bold;
    letter-spacing: 1px;
    box-shadow: 0 1px 3px rgba(0, 0, 0, 0.12), 0 1px 2px rgba(0, 0, 0, 0.24);
}


</style>