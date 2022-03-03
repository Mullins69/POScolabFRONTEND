<template>
  <section id="nav">
    <header>
      <div class="nav-wrapper">
        <div class="logo-container">
          <img class="logo" src="../assets/logo/logo.png" alt="Logo" />
        </div>
        <nav>
          <input class="hidden" type="checkbox" id="menuToggle" />
          <label class="menu-btn" for="menuToggle">
            <div class="menu"></div>
            <div class="menu"></div>
            <div class="menu"></div>
          </label>
          <div class="nav-container">
            <ul class="nav-tabs">
              <li class="nav-tab"><router-link to="/">Home</router-link></li>
              <li class="nav-tab">
                <router-link :to="{ name: 'Products' }">Products</router-link>
              </li>
              <li class="nav-tab">
                <router-link :to="{ name: 'CreateProduct' }"
                  >Create Product</router-link
                >
              </li>
              <li class="nav-tab">
                <router-link
                  @click="logout"
                  class="logout"
                  :to="{ name: 'Home' }"
                  >Log out</router-link
                >
              </li>
              <li class="nav-tab">
                <router-link :to="{ name: 'Login' }">Login</router-link>
              </li>
              <li class="nav-tab"><router-link :to="{ name: 'UserProfile'}">
              User Profile</router-link></li>
            </ul>
            
          </div>
        </nav>
        
      </div>
    </header>
  </section>
</template>
<script>
export default {
  data() {
    return {
      // isActive: false,
    };
  },
  methods: {
    logout() {
      localStorage.clear();
    },
  },
};
</script>
<style scoped>
li a {
    text-decoration: none;
	color: white;
}
li a:hover {
    text-decoration: none;
	color: yellow;
}
h1 {
  margin:0;
  color: #fff;
}

.center {
  text-align: center;
}

.nav-wrapper {

  display: flex;
  flex-direction: row;
  flex-wrap: nowrap;
  align-items: center;
  justify-content: space-between;
  margin: 0;
  width: 100%  !important;
  height: 60px;
  overflow: hidden;
  padding: 0 25px;
  z-index: 2;
  background: #343a40;
  box-shadow: 0 1px 2px rgba(0, 0, 0, 0.9);
}



.logo-container {
  display: flex;
  justify-content: center;
  align-items: center;
}

.logo {
  height: 60px;
  width: 80px;
}

.nav-container{
  color: #eee;
}

.nav-tabs {
  color: white;
  display: flex;
  font-weight: 600;
  font-size: 18px;
  list-style: none;
}

.nav-tab:not(:last-child) {
  padding: 10px 25px;
  margin: 0;
  border-right: 1px solid #eee;
}

.nav-tab:last-child {
  padding: 10px 0 0 25px;
}

.nav-tab,
.menu-btn {
  cursor: pointer;
}

.hidden {
  display: none;
}

@media screen and (max-width: 800px) {
  .nav-container {
    position: fixed;
    display: none;
    overflow-y: auto;
    z-index: -1;
    top: 0;
    right: 0;
    width: 280px;
    height: 100%;
    background: #343a40;
    box-shadow: -1px 0 2px rgba(0, 0, 0, 0.2);
  }

  .nav-tabs {
    flex-direction: column;
    align-items: flex-end;
    margin-top: 80px;
    width: 100%;
  }

  .nav-tab:not(:last-child) {
    padding: 20px 25px;
    margin: 0;
    border-right: unset;
    border-bottom: 1px solid #f5f5f5;
  }

  .nav-tab:last-child {
    padding: 15px 25px;
  }

  .menu-btn {
    position: relative;
    display: block;
    margin: 0;
    width: 20px;
    height: 15px;
    cursor: pointer;
    z-index: 2;
    padding: 10px;
    border-radius: 10px;
  }

  .menu-btn .menu {
    display: block;
    width: 100%;
    height: 2px;
    border-radius: 2px;
    background: #111;
  }

  .menu-btn .menu:nth-child(2) {
    margin-top: 4px;
    opacity: 1;
  }

  .menu-btn .menu:nth-child(3) {
    margin-top: 4px;
  }

  #menuToggle:checked + .menu-btn .menu {
    transition: transform 0.2s ease;
  }

  #menuToggle:checked + .menu-btn .menu:nth-child(1) {
    transform: translate3d(0, 6px, 0) rotate(45deg);
  }

  #menuToggle:checked + .menu-btn .menu:nth-child(2) {
    transform: rotate(-45deg) translate3d(-5.71429px, -6px, 0);
    opacity: 0;
  }

  #menuToggle:checked + .menu-btn .menu:nth-child(3) {
    transform: translate3d(0, -6px, 0) rotate(-45deg);
  }

  #menuToggle:checked ~ .nav-container {
    z-index: 1;
    display: flex;
    animation: menu-slide-left 0.3s ease;
  }
  @keyframes menu-slide-left {
    0% {
      transform: translateX(200px);
    }
    to {
      transform: translateX(0);
    }
  }
}
</style>
