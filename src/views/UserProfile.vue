<template>
  <section class="create" v-if="renderComponent">
    <div class="container">
      <div class="form-body">
        <div class="row">

                <div class="col" v-if="users">
                  <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/7/7e/Circle-icons-profile.svg/1200px-Circle-icons-profile.svg.png" alt="">
                  <span class="del">
                  <h3>Name: {{users.fullname}} </h3>
                  <h3>Email: {{users.email}} </h3>
                  <h3>Cell No: {{users.phone_number}} </h3>
                  </span>

                </div>  
      
        </div>
        <div class="row">
          <div class="col">
            <!-- Button trigger modal -->
          <span>
          <button type="button" class="btn btn-primary" data-bs-toggle="modal" data-bs-target="#exampleModal">
                      Edit Profile
            </button></span>
                  <span class="del"><button type="button" class="btn btn-danger" @click.prevent="deleteUser">
                      Delete User
              </button>
            </span>

<!-- Modal -->
              <div class="modal fade" id="exampleModal" tabindex="-1" aria-labelledby="exampleModalLabel" aria-hidden="true">
                <div class="modal-dialog">
                    <div class="modal-content">
                     <div class="modal-header">
                        <h5 class="modal-title" id="exampleModalLabel">Edit Your Details Below</h5>
        
                   <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
                 </div>
                   <div class="modal-body">
               <form
                  class="requires-validation"
                  @submit.prevent="modUser"
                >
                  <div class="col-md-12">
                    <input
                      type="text"
                      placeholder="Name"
                      v-model="Name"
                    /> <br>
                  </div>

                  <div class="col-md-12">
                      <label for="">Email</label><br>
                    <input

                      type="text"
                      placeholder="Email"

                      v-model="Email"
                    /> <br>
                  </div>
                  <div class="col-md-12">
                      <label for="">Password</label><br>
                    <input

                      type="text"
                      v-model="Password"
                      placeholder="Password"

                    /> 
                  </div> <br>
                 
                  <div class="col-md-12">
                      <label for="">Phone number</label><br>
                    <input

                      type="text"
                      placeholder="Phone number"
                      v-model="number"
                    />
                  </div>
                  <div class="form-button mt-3">
                    <button id="submit" type="submit" data-bs-dismiss="modal"  class="btn btn-primary">
                      Edit
                    </button>
                  </div>
                </form> 
      </div>
      <div class="modal-footer">

      </div>
    </div>
  </div>
</div>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>
<script>


export default {

  data() {

    return {
      users: null,
      Name: "",
      Email: "",
      Password: "",
      number: "",
      renderComponent: true,
      
    };
  },
methods: {
  deleteUser(){
    if(confirm("Do you really want to delete your profile?")){
            if (!localStorage.getItem("jwt")) {
        alert("User not logged in");
        return this.$router.push({ name: "Login" });
      }
      fetch('https://pos-colab.herokuapp.com/users', {
      method: 'DELETE',
        headers: {
          "Content-type": "application/json; charset=UTF-8",
          Authorization: `Bearer ${localStorage.getItem("jwt")}`,
        },
      })
        .then((response) => response.json())
        .then((json) => {
          alert("DELETED USER")
          localStorage.clear();
          return this.$router.push({ name: "Home" });
        })
        .catch((err) => {
          alert(err);
        });
            }},
  forceRerender() {
        // Removing my-component from the DOM
        this.renderComponent = false;

        this.$nextTick(() => {
          // Adding the component back in
          this.renderComponent = true;
        });
      }
    ,
  modUser(){
      if (!localStorage.getItem("jwt")) {
        alert("User not logged in");
        return this.$router.push({ name: "Login" });
      }
      fetch("https://pos-colab.herokuapp.com/users/", {
        method: "PUT",
        body: JSON.stringify({
          fullname: this.Name,
          email: this.Email,
          password: this.Password,
          phone_number: this.number
        }),
        headers: {
          "Content-type": "application/json; charset=UTF-8",
          Authorization: `Bearer ${localStorage.getItem("jwt")}`,
        },
      })
        .then((response) => response.json())
        .then((json) => {
          alert("User Updated");
          this.$router.push({ name: "UserProfile" });
        })
        .catch((err) => {
          alert(err);
        });
  }
  
    },
    mounted(){
      if (!localStorage.getItem("jwt")) {
        alert("User not logged in");
        return this.$router.push({ name: "Login" });
      }
      fetch("https://pos-colab.herokuapp.com/users/oneuser/", {
        method: "GET",
        headers: {
          "Content-type": "application/json; charset=UTF-8",
          Authorization: `Bearer ${localStorage.getItem("jwt")}`,
        },
      })
        .then((response) => response.json())
        .then((json) => {
          this.users = json
        })
        .catch((err) => {
          alert(err);
        });
    },
    
    
    
};
</script>
<style scoped>
@import url("https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;500;700;900&display=swap");
.dess{
  padding-top: 20px;
}

.container{
  margin-top: -250px !important;

}
.modal-title{
  color: black;
}
.create {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  text-align: center;
  min-height: 100vh;
  background-color: #212529 !important;
  overflow-x: hidden;

}

.form-items{
    margin-top: 0px;
}
.form-holder .form-content {
  position: relative;
  text-align: center;
  display: -webkit-box;
  display: -moz-box;
  display: -ms-flexbox;
  display: -webkit-flex;
  display: flex;
  -webkit-justify-content: center;
  justify-content: center;
  -webkit-align-items: center;
  align-items: center;
  padding: 60px;
}

.form-content .form-items {
  border: 3px solid #fff;
  padding: 40px;
  display: inline-block;
  width: 100%;

  -webkit-border-radius: 10px;
  -moz-border-radius: 10px;
  border-radius: 10px;
  text-align: left;
  -webkit-transition: all 0.4s ease;
  transition: all 0.4s ease;
}

.form-content h3 {
  color: #fff;
  text-align: left;
  font-size: 28px;
  font-weight: 600;
  margin-bottom: 5px;
}

.form-content h3.form-title {
  margin-bottom: 30px;
}

.form-content p {
  color: #fff;
  text-align: left;
  font-size: 17px;
  font-weight: 300;
  line-height: 20px;
  margin-bottom: 30px;
}

.form-content label,
.was-validated .form-check-input:invalid ~ .form-check-label,
.was-validated .form-check-input:valid ~ .form-check-label {
  color: #fff;
}

.form-content input[type="text"],
.form-content select {
  width: 100%;
  padding: 9px 20px;
  text-align: left;
  border: 0;
  outline: 0;
  border-radius: 6px;
  background-color: #fff;
  font-size: 15px;
  font-weight: 300;
   color: black;
  -webkit-transition: all 0.3s ease;
  transition: all 0.3s ease;
  margin-top: 16px;
}

.btn-primary {
  background-color: #6c757d;
  outline: none;
  border: 0px;
  box-shadow: none;
}

.btn-primary:hover,
.btn-primary:focus,
.btn-primary:active {
  background-color: #495056;
  outline: none !important;
  border: none !important;
  box-shadow: none;
}

.form-content textarea {
  position: static !important;
  width: 100%;
  padding: 8px 20px;
  border-radius: 6px;
  text-align: left;
  background-color: #fff;
  border: 0;
  font-size: 15px;
  font-weight: 300;
  color: black;
  outline: none;
  resize: none;
  height: 120px;
  -webkit-transition: none;
  transition: none;
  margin-bottom: 14px;
}

.form-content textarea:hover,
.form-content textarea:focus {
  border: 0;
  background-color: #ebeff8;
  color: #8d8d8d;
}

.mv-up {
  margin-top: -9px !important;
  margin-bottom: 8px !important;
}

img{
    height: 150px;
    width: 180px;
    margin: 9px;
}

label{
    font-size: 18px;
}

.btn{
    width: 150px;
    border-radius: 350px;
}

.btn:hover {
  transform: scale(1.1);
}
.del{
  margin-left:20px;
  
}

.row{
  margin-top: 10px;
}
</style>
