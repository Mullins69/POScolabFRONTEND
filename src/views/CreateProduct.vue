<template>
<section class="crud">
<form @submit.prevent="createBlog" class="form neu-border">
    <h2 class="form-heading">Create a Product</h2>
    <input
      class="form-input neu-border-inset"
      type="text"
      v-model="title"
      placeholder="Title"
      required
    />
    <input
      class="form-input neu-border-inset"
      type="text"
      v-model="category"
      placeholder="Category"
      required
    />
    <input
      class="form-input neu-border-inset"
      type="text"
      v-model="img"
      placeholder="Blog Image"
      required
    />
    <textarea
      class="form-input neu-border-inset"
      type="text"
      v-model="description"
      placeholder="Description"
      required
    ></textarea>
  <input
      class="form-input neu-border-inset"
      type="text"
      v-model="price"
      placeholder="price"
      required
    />
    <button type="submit" @click="createProduct" class="form-btn neu-border">Create Product</button>
  </form>
</section>
  
</template>
<script>
export default {
  data() {
    return {
      title: "",
      category: "",
      description: "",
      img: "",
      price:""
    };
  },
  methods: {
    createProduct() {
      if (!localStorage.getItem("jwt")) {
        alert("User not logged in");
        return this.$router.push({ name: "Login" });
      }
      fetch("https://pos-colab.herokuapp.com/products", {
        method: "POST",
        body: JSON.stringify({
          title: this.title,
          description: this.description,
          category: this.category,
          price: this.price,
          img: this.img,
        }),
        headers: {
          "Content-type": "application/json; charset=UTF-8",
          Authorization: `Bearer ${localStorage.getItem("jwt")}`,
        },
      })
        .then((response) => response.json())
        .then((json) => {
          alert("Product Created");
          this.$router.push({ name: "Products" });
        })
        .catch((err) => {
          alert(err);
        });
    },
  },
};
</script>
<style scoped>

.crud{
  background-color: #212529  !important;
  height: 100vh;
}



.neu-border {
  border-radius: 30px;
  background: #6c757d;
  box-shadow: 8px 8px 15px #e4e4e4, -8px -8px 15px #dee2e6;
}
.neu-border-inset {
  border-radius: 30px;
  background: #f5f5f5;
  box-shadow: inset 8px 8px 15px #e4e4e4, inset -8px -8px 15px #ffffff;
}

.form {
  display: flex;
  flex-direction: column;
  justify-content: center;
  padding: 40px;
  padding-top: 9  0px;
  gap: 20px;
  width: 100%;
  max-width: 600px;
  margin-inline: auto;
}

.form-heading {
  text-align: center;
  text-transform: uppercase;
}

.form-input,
.form-btn {
  border: none;
  outline: none;
  padding: 20px;
}

.form-btn {
  cursor: pointer;
  transition: all 0.1s linear;
}

.form-btn:hover {
  transform: scale(1.05);
}

.form-social-login {
  display: flex;
  justify-content: space-between;
}

.form-social-btn {
  width: 45%;
  color: #333;
}
</style>
