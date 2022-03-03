<template>
<section v-if="renderComponent">
  <div v-if="products" class="products" >
    <h2>Products</h2>
    <div class="right-side">
      <button class="cart">
        <router-link :to="{ name: 'Cart' }"> CART</router-link>
      </button>
    </div>
    <div class="container-fluid" style="position: relative" >
      <div class="row">
        <div class="col">
           <div class="search-box">
      <input type="text" v-model="search" class="search-input" placeholder="Search..">
   </div>
        </div>
      </div>
      <div class="row row-cols-1 row-cols-xs-2 row-cols-sm-2 row-cols-lg-4 g-3">
        <div class="col hp" v-for="product of filterProducts" :key="product._id">
          <div class="card h-100 shadow-sm">
            <a href="#">
              <img
                :src="product.img"
                class="card-img-top"
                :alt="product.title"
              />
            </a>

            <div class="label-top shadow-sm">
              <span class="text-white" >{{ product.category }}</span>
            </div>
            <div class="card-body">
              <div class="clearfix mb-3">
                <span class="float-start badge rounded-pill bg-success"
                  >R{{ product.price }}</span
                >

                <span class="float-end"></span>
              </div>
              <h5 class="card-title">
                <a target="_blank" href="#">{{ product.title }}</a>
              </h5>
              <h5>
                {{ product.description }}
              </h5>
              <div class="d-grid gap-2 my-4">
                <span class="btn btn-warning bold-btn" @click="addToCart">add to cart</span>
              </div>
              <div class="clearfix mb-1">
                <button class="float-end btn btn-danger" @click="DeleteProduct" >DELETE</button>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
  <div v-else>Loading Products...</div>
</section>
</template>
<script>
export default {
  data() {
    return {
      products: null,
      search: "",
      renderComponent: true,
    };
  },
  methods: {
    forceRerender() {
        // Removing my-component from the DOM
        this.renderComponent = false;

        this.$nextTick(() => {
          // Adding the component back in
          this.renderComponent = true;
        });
      }
    ,
    addToCart(){
        fetch("https://pos-colab.herokuapp.com/users", {
        method: "POST",
        body: JSON.stringify({
          fullname: this.name,
          email: this.email,
          phone_number: this.contact,
          password: this.password,
        }),
        headers: {
          "Content-type": "application/json; charset=UTF-8",
        },
      })
        .then((response) => response.json())
        .then((json) => {
          alert("User registered");
          localStorage.setItem("jwt", json.jwt);
          this.$router.push({ name: "Products" });
        })
        .catch((err) => {
          alert(err);
        });
    }
  }
  ,
  mounted() {
    if (localStorage.getItem("jwt")) {
      fetch("https://pos-colab.herokuapp.com/products/", {
        method: "GET",
        headers: {
          "Content-type": "application/json; charset=UTF-8",
          Authorization: `Bearer ${localStorage.getItem("jwt")}`,
        },
      })
        .then((response) => response.json())
        .then((json) => {
          this.products = json;
          this.products.forEach(async (product) => {
            await fetch(
              "https://pos-colab.herokuapp.com/products/ " + product.created_by,
              {
                method: "GET",
                headers: {
                  "Content-type": "application/json; charset=UTF-8",
                  Authorization: `Bearer ${localStorage.getItem("jwt")}`,
                },
              }
            )
              .then((response) => response.json())
              .then((json) => {
                product.created_by = json.title;
              });
          });
        })
        .catch((err) => {
          alert("User not logged in");
        });
    } else {
      alert("User not logged in");
      this.$router.push({ name: "Login" });
    }
  },
  computed:{
    filterProducts:function(){
      return this.products.filter((product) =>{
        return product.title.match(this.search)
      })
    }
  }
};
</script>
<style scoped>
@import url("https://fonts.googleapis.com/css2?family=Fira+Sans+Extra+Condensed:ital,wght@0,100;0,200;0,300;0,400;0,500;0,600;0,700;0,800;0,900;1,100;1,200;1,300;1,400;1,500;1,600;1,700;1,800;1,900&display=swap");
@import url("https://fonts.googleapis.com/css2?family=Heebo:wght@100;200;300;400;500;600;700;800;900&display=swap");
@import url("https://fonts.googleapis.com/css2?family=Roboto:ital,wght@0,100;0,300;0,400;0,500;0,700;0,900;1,100;1,300;1,400;1,500;1,700;1,900&display=swap");
@import url("https://use.fontawesome.com/releases/v5.13.0/css/all.css");

:root {
  --font1: "Heebo", sans-serif;
  --font2: "Fira Sans Extra Condensed", sans-serif;
  --font3: "Roboto", sans-serif;

  --btnbg: #ffcc00;
  --btnfontcolor: rgb(61, 61, 61);
  --btnfontcolorhover: rgb(255, 255, 255);
  --btnbghover: #ffc116;
  --btnactivefs: rgb(241, 195, 46);

  --label-index: #960796;
  --danger-index: #5bc257;
  /* PAGINATE */
  --link-color: #000;
  --link-color-hover: #fff;
  --bg-content-color: #ffcc00;
}
.products{
  height: fit-content;
}
.search-box{
  width: 100%;
  position: relative;
  display: flex;

}
.search-input{
  width: 100%;
  padding: 10px;
  border: 4px solid #111d5e;
  border-radius:10px 0 0 10px ;
  border-right: none;
  outline: none;
  font-size: 20px;
  color: tomato;
  background: none;
}
.search-button{
 text-align: center;
height: 51px;
width: 40px;
outline: none;
cursor: pointer;
border: 4px solid #111d5e;
 border-radius: 0 10px 10px 0 ;
border-left: none;
background: none;
font-size: 20px;
border-left: 4px solid #111d5e;


}
.search{
  width: 35%;
  position: absolute;
  left: 40%;
  top: 40%;
  transform: translate(-50% , -50%);
}
.container-fluid {
  margin-top: 60px;
  max-width: 1400px;
}

.products {
  background-color: #212529 !important;
}

.card {
  background: #6c757d;
  box-shadow: 0 6px 10px rgba(0, 0, 0, 0.08), 0 0 6px rgba(0, 0, 0, 0.05);
  transition: 0.3s transform cubic-bezier(0.155, 1.105, 0.295, 1.12),
    0.3s box-shadow,
    0.3s -webkit-transform cubic-bezier(0.155, 1.105, 0.295, 1.12);
  border: 0;
  border-radius: 1rem;
}

.label-top{
  background-color: #960796 !important;

}

.card:hover {
  background-color: white;
  color: #000;
  font-weight: bold;
}

.card-img,
.card-img-top {
  border-top-left-radius: calc(1rem - 1px);
  border-top-right-radius: calc(1rem - 1px);
}

.card h5 {
  overflow: hidden;
  height: 55px;
  font-weight: 300;
  font-size: 1rem;
}

.card h5 a {
  color: black;
  text-decoration: none;
}

.card-img-top {
  width: 100%;
  min-height: 250px;
  max-height: 250px;
  object-fit: contain;
  padding: 30px;
}

.card h2 {
  font-size: 1rem;
}

.card:hover {
  transform: scale(1.02);
  box-shadow: 0 10px 20px rgba(0, 0, 0, 0.12), 0 4px 8px rgba(0, 0, 0, 0.06);
}

/* Centered text */
.label-top {
  position: absolute;
  background-color: var(--label-index);
  color: #fff;
  top: 8px;
  right: 8px;
  padding: 5px 10px 5px 10px;
  font-size: 0.7rem;
  font-weight: 600;
  border-radius: 3px;
  text-transform: uppercase;
}

.top-right {
  position: absolute;
  top: 24px;
  left: 24px;

  width: 90px;
  height: 90px;
  border-radius: 50%;
  font-size: 1rem;
  font-weight: 900;
  background: #8bc34a;
  line-height: 90px;
  text-align: center;
  color: white;
}

.top-right span {
  display: inline-block;
  vertical-align: middle;
  /* line-height: normal; */
  /* padding: 0 25px; */
}

.aff-link {
  /* text-decoration: overline; */
  font-weight: 500;
}

.over-bg {
  background: rgba(53, 53, 53, 0.85);
  box-shadow: 0 8px 32px 0 rgba(31, 38, 135, 0.37);
  backdrop-filter: blur(0px);
  -webkit-backdrop-filter: blur(0px);
  border-radius: 10px;
}
.bold-btn {
  background-color: #ffcc00 !important;
  color: #000 !important;
  font-size: 1rem;
  font-weight: 500;
  text-transform: uppercase;
  padding: 5px 50px 5px 50px;
}
.box .btn {
  font-size: 1.5rem;
}

@media (max-width: 1025px) {
  .btn {
    padding: 5px 40px 5px 40px;
  }
}
@media (max-width: 250px) {
  .btn {
    padding: 5px 30px 5px 30px;
  }
}

/* START BUTTON */
.btn-warning {
  background: var(--btnbg);
  color: var(--btnfontcolor);
  fill: #ffffff;
  border: none;
  text-decoration: none;
  outline: 0;
  /* box-shadow: -1px 6px 19px rgba(247, 129, 10, 0.25); */
  border-radius: 100px;
}
.btn-warning:hover {
  background: var(--btnbghover);
  color: var(--btnfontcolorhover);
  /* box-shadow: -1px 6px 13px rgba(255, 150, 43, 0.35); */
}
.btn-check:focus + .btn-warning,
.btn-warning:focus {
  background: var(--btnbghover);
  color: var(--btnfontcolorhover);
  /* box-shadow: -1px 6px 13px rgba(255, 150, 43, 0.35); */
}
.btn-warning:active:focus {
  box-shadow: 0 0 0 0.25rem var(--btnactivefs);
}
.btn-warning:active {
  background: var(--btnbghover);
  color: var(--btnfontcolorhover);
  /* box-shadow: -1px 6px 13px rgba(255, 150, 43, 0.35); */
}

/* END BUTTON */

.bg-success {
  font-size: 1rem;
  background-color: #ffcc00 !important;
  color: #000 !important;
}
.bg-danger {
  font-size: 1rem;
}

.price-hp {
  font-size: 1rem;
  font-weight: 600;
  color: darkgray;
}

.amz-hp {
  font-size: 0.7rem;
  font-weight: 600;
  color: darkgray;
}

.fa-question-circle:before {
  /* content: "\f059"; */
  color: darkgray;
}

.fa-heart:before {
  color: crimson;
}
.fa-chevron-circle-right:before {
  color: darkgray;
}

.right-side {
  justify-content: center;
  align-items: center;
  position: fixed;
  z-index: 10;
}

.cart {
  background-color: #ffcc00;
  position: sticky;
  color: #000;
  border-radius: 300px;
  border: none;
  width: 100px;
  height: 40px;

}

.cart:hover {
  transform: scale(1.1);
  color: white;
}

.profile {
  background-color: #ffcc00;
  color: #000;
  position: sticky;
  border-radius: 300px;
  border: none;
  width: 180px;
  height: 40px;
  margin: 10px;
}

.profile:hover {
  color: white;
  transform: scale(1.1);
}
</style>
