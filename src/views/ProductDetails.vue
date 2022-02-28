<template>
  <div v-if="product">
    <div class="blog">
      <img class="blog-image neu-border"  :src="product.img" :alt="product.title" />
      <div class="blog-details">
        <h2>{{ product.title }}</h2>
        <h4>{{ product.fullname }} - {{ product.category }}</h4>
        <p>{{ product.description }}</p>
      </div>
    </div>
  </div>
  <div v-else>Loading the product...</div>
</template>
<script>
export default {
  props: ["id"],
  data() {
    return {
      product: null,
    };
  },
  mounted() {
    fetch("https://pos-colab.herokuapp.com/products/" + this._id, {
      method: "GET",
      headers: {
        "Content-type": "application/json; charset=UTF-8",
        Authorization: `Bearer ${localStorage.getItem("jwt")}`,
      },
    })
      .then((response) => response.json())
      .then(async (json) => {
        this.product = json;
        await fetch(
          "https://pos-colab.herokuapp.com/users/" + json._id,
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
  },
};
</script>
<style>
.blog {
  display: flex;
  flex-direction: column;
  padding: 0 10%;
  margin-inline: auto;
  align-items: stretch;
  gap: 30px;
}
.blog-image {
  padding: 10px;
  width: 100%;
  min-width: 100%;
  max-height: 50vh;
  object-fit: cover;
}
.blog-details {
  display: flex;
  flex-direction: column;
  justify-content: center;
  text-align: left;
  gap: 8px;
}

.blogs-container {
  display: flex;
  flex-wrap: wrap;
  width: 100%;
  margin-inline: auto;
  padding: 30px;
  gap: 2%;
  justify-content: stretch;
  align-items: stretch;
}
.neu-border {
  border-radius: 30px;
  background: #f5f5f5;
  box-shadow: 8px 8px 15px #e4e4e4, -8px -8px 15px #ffffff;
}
.neu-border-inset {
  border-radius: 30px;
  background: #f5f5f5;
  box-shadow: inset 8px 8px 15px #e4e4e4, inset -8px -8px 15px #ffffff;
}
@media screen and (max-width: 500px) {
  .blog {
    flex-direction: column;
  }
  .blog-image,
  .blog-details {
    width: 100%;
  }

  .blog-details {
    align-items: flex-start;
    text-align: left;
  }
}
</style>
