<template>
  <div id="app">
    <header>
      <div class="container-fluid">
        <div class="row jc-between">
          <img src="@/assets/aerolab-logo.svg" alt="Aerolab" />
          <div class="user">
            <div class="user-name color-dark-gray">{{ this.user }}</div>
            <div class="user-points color-dark-gray">
              <span :class="{ 'ani-number': animated }" 
                @animationend="animated = false">
								{{ this.userPoints }}
              </span>
              <img class="icon icon-coin ml-1" src="@/assets/icons/coin.svg" alt="coin"/>
            </div>
					<button class="pill reload" @click="reloadPoints()"> Reload </button>
          </div>
        </div>
      </div>
    </header>
    <section class="hero">
      <div class="container">
        <div class="row">
          <h1>Electronics</h1>
        </div>
      </div>
    </section>
    <main>
      <section class="filters">
        <div class="container">
          <div class="row border-bottom">
            <div class="total-prod color-dark-gray">
              {{ pageOffest }} of {{ totalResults }} products
            </div>
            <div class="sort-by">
              <span>Sort by:</span>
              <button
                @click="(orderBy = ''), (active = 1), getProducts()"
                :class="[active == 1 ? 'active' : '']"
                class="pill pill-filter">
                Most recent
              </button>
              <button
                @click="(orderBy = 'lowPrices'), (active = 2)"
                :class="[active == 2 ? 'active' : '']"
                class="pill pill-filter">
                Lowest price
              </button>
              <button
                @click="(orderBy = 'hightPrices'), (active = 3)"
                :class="[active == 3 ? 'active' : '']"
                class="pill pill-filter">
                Highest price
              </button>
            </div>
          </div>
        </div>
        <section class="products">
          <div class="container">
            <div class="prod-grid">
              <div v-for="item in productsFiltered" v-bind:key="item.id" class="prod" :id="item._id">
                <div class="prod-status">
                  <div v-if="userPoints > item.cost" class="icon-redeem">
                    <img src="@/assets/icons/buy-blue.svg" alt="redeem"/>
                    <img src="@/assets/icons/buy-white.svg" alt="redeem"/>
                  </div>
                  <div v-else>
                    <div class="pill pill-prod">
                      You need {{ item.cost - userPoints }}
                      <img class="icon icon-coin ml-1" src="@/assets/icons/coin.svg" alt="coin"/>
                    </div>
                  </div>
                </div>
                <img class="img-fluid prod-img" :src="item.img.url" :alt="item.name"/>
                <div class="prod-info">
                  <div class="d-flex jc-between">
                    <div class="prod-cat">{{ item.category }}</div>
                    <div class="color-dark-gray">{{ item.cost }}</div>
                  </div>
                  <div class="prod-title color-dark-gray">{{ item.name }}</div>
                </div>
                <div class="prod-price">
                  <div class="price">
                    {{ item.cost }}
                    <img class="icon icon-coin ml-1" src="@/assets/icons/coin.svg" alt="coin"/>
                  </div>
                  <a v-show="userPoints > item.cost" @click="redeemProd(item._id)" class="btn-redeem">
                    Redeem now
                  </a>
                </div>
              </div>
            </div>
            <div class="container">
              <div class="row jc-center">
                <button class="btn-primary mb-4" 
                  @click="loadMore"
                  v-if="currentPage * maxPerPage < products.length">
                  Load more
                </button>
              </div>
            </div>
          </div>
        </section>
      </section>
    </main>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "products",
  data() {
    return {
      user: "",
      userPoints: "",
      products: [],
      orderBy: "",
      active: "1",
      currentPage: 1,
      maxPerPage: 16,
      showReadMore: true,
      animated: false,
      sure: false,
    };
  },
  created() {
    this.token ="eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJfaWQiOiI2MDUxMjU0OTdlNzE4NzAwMjBlMzhmOTUiLCJpYXQiOjE2MTU5MzA2OTd9.37CNui965OoX0h5MeRCg52SXA7-yEbJkfmRxtLjmSQE";
    this.getUser();
    this.getProducts();
  },
  methods: {
    getUser() {
      axios
        .get("https://coding-challenge-api.aerolab.co/user/me", {
          headers: {
            Authorization: `Bearer ${this.token}`,
          },
        })
        .then((res) => {
          this.user = res.data.name;
          this.userPoints = res.data.points;
          console.log(res.data);
        })
        .catch((e) => console.log(e));
    },
    getProducts() {
      axios
        .get("https://coding-challenge-api.aerolab.co/products", {
          headers: {
            Authorization: `Bearer ${this.token}`,
          },
        })
        .then((res) => {
          this.products = res.data;
        })
        .catch((e) => console.log(e));
    },
    redeemProd(id) {
      axios
        .post("https://coding-challenge-api.aerolab.co/redeem",
          {
            productId: id,
          },
          {
            headers: {
              Authorization: `Bearer ${this.token}`,
            },
          }
        )
        .then(function (response) {
          console.log(response);
        })
        .finally(() => {
          this.getUser();
        });
    },
		reloadPoints(){
				axios
        .post("https://coding-challenge-api.aerolab.co/user/points",
          {
            'amount': 1000
          },
          {
            headers: {
              Authorization: `Bearer ${this.token}`,
            },
          }
        )
        .then(function (response) {
          console.log(response);
        })
        .finally(() => {
          this.getUser();
        });
		},
    loadMore() {
      this.currentPage += 1;
    }
	},
  computed: {
    totalResults() {
      return Object.keys(this.products).length;
    },
    pageCount() {
      return Math.ceil(this.totalResults / this.maxPerPage);
    },
    pageOffest() {
      return this.maxPerPage * this.currentPage;
    },
    paginatedProd() {
      return this.products.slice(0, this.currentPage * this.maxPerPage);
    },
    productsFiltered() {
      let filterProd = this.paginatedProd;
      if (this.orderBy === "lowPrices") {
        filterProd.sort(function (a, b) {
          return parseFloat(a.cost) - parseFloat(b.cost);
        });
      } else if (this.orderBy === "hightPrices") {
        filterProd.sort(function (a, b) {
          return parseFloat(b.cost) - parseFloat(a.cost);
        });
      }
      return filterProd;
    },
  },
  watch: {
    userPoints: function () {
      this.animated = true;
    },
  },
};
</script>

<style lang="scss">
//Variables
:root {
  --main-blue-rgb: 10, 212, 250;
  --gray-800-rgb: 97, 97, 97;
  --main-blue: #0ad4fa;
  --color-accent: #f67b04;
  --gray-800: #616161;
  --gray-500: #bbbbbb;
  --gray-400: #d9d9d9;
  --gray-300: #ededed;
}

//  base

* {
  margin: 0;
  box-sizing: border-box;
}
body {
  color: #a3a3a3;
  background-color: #f9f9f9;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

.container {
  width: 100%;
  max-width: 1120px;
  padding: 0 16px;
  margin-right: auto;
  margin-left: auto;
}

.container-fluid {
  width: 100%;
  padding: 0 16px;
}

.row {
  display: flex;
  // margin-right: -16px;
  // margin-left: -16px;
}

header {
  //height: 80px;
  padding: 8px 0;
  background-color: #fff;
  position: fixed;
  top: 0;
  width: 100%;
  box-shadow: 0 2px 10px rgb(0 0 0 / 9%);
  z-index: 2;
}

// user
.user {
  display: flex;
  align-items: center;
  justify-content: flex-end;
  min-width: 180px;
  font-size: 1rem;

  &-points {
    display: flex;
    align-items: center;
    margin: 4px 0 4px 16px;
    padding: 4px 12px;
    border-radius: 18px;
    background-color: var(--gray-300);
  }
}
.hero {
  background-image: url("assets/header-x1.png");
  background-size: cover;
  height: 16vh;
  display: flex;
  align-items: flex-end;
  margin-top: 64px;

  h1 {
    color: #fff;
    font-weight: 900;
    margin-bottom: 24px;
  }

  @media (min-width: 768px) {
    height: 28vh;
    h1 {
      font-size: 3rem;
      margin-bottom: 48px;
    }
  }
  @media (min-width: 1366px) {
    height: 420px;
    h1 {
      font-size: 4rem;
    }
  }
}

.filters {
  .row {
    padding: 1.2rem 0px;
    align-items: center;
  }

  .sort-by {
    display: flex;
    align-items: center;
    color: var(--gray-800);

    @media (min-width: 768px) {
      margin-left: 1rem;
      padding-left: 1rem;
      border-left: 1px solid var(--gray-500);
    }
  }

  .total-prod {
    display: none;
    @media (min-width: 768px) {
      display: block;
    }
  }
}

.pill {
  display: flex;
  align-items: center;
  background-color: var(--gray-300);
  border-radius: 14px;
  padding: 8px 16px;
  font-size: 0.8rem;
  font-weight: 800;
	border: none;
	outline: none;
  margin: 0 4px;

  &.active {
    background-color: var(--main-blue);
    color: #fff;
  }

  &-prod {
    background-color: rgba(var(--gray-800-rgb), 0.6);
    color: #fff;
    border-radius: 22px;

    .icon-coin {
      width: 25px;
      height: 25px;
    }
  }

  &-filter {
    cursor: pointer;
    border: 0;
    outline: 0;
    transition: 0.15s;
    color: var(--gray-800);
    @media (min-width: 768px) {
      font-size: 1.1rem;
      font-weight: 400;
      padding: 12px 28px;
      border-radius: 22px;
    }
    &:hover {
      background-color: var(--color-accent);
      color: #fff;
    }
  }
	&.reload {
		font-size:12px;
		font-weight: 400;
    color:var(--gray-800);  
		cursor: pointer;
		&:hover {
      background-color: var(--color-accent);
      color: #fff;
    }
	}
}

.prod-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, 280px);
  grid-gap: 48px;
  justify-content: center;
  padding: 42px 0;
}

.prod {
  display: inline-flex;
  flex-direction: column;
  justify-content: space-between;
  position: relative;
  overflow: hidden;
  background-color: #fff;
  box-shadow: 2px 2px 6px 0 rgba(0, 0, 0, 0.2);
  padding: 18px;
  margin: 0 auto;
  width: 280px;
  height: 280px;
  transition: 0.35s;
  cursor: pointer;

  &-status {
    position: absolute;
    right: 12px;
    top: 12px;
    z-index: 1;

    .icon-redeem img:first-child {
      z-index: 2;
      position: absolute;
      top: 2px;
      right: 6px;
      transition: 0.35s;
    }
  }

  &-img {
    margin-bottom: 1rem;
    border-bottom: 1px solid var(--gray-400);
  }

  &-price {
    position: absolute;
    top: 0;
    bottom: 0;
    left: 0;
    right: 0;
    background-image: linear-gradient( -180deg, rgb(10 212 250 / 76%) 0%, rgb(37 187 241 / 76%) 100% );
    transition: 0.35s;
    transform: translateY(-100%);
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;

    .price {
      display: flex;
      align-items: center;
      justify-content: center;
      color: #fff;
      font-size: 2rem;
      font-weight: 500;
      margin-bottom: 12px;
      transform: translateY(-100%);
      transition: 0.65s;
    }
    .btn-redeem {
      background-color: #fff;
      min-width: 200px;
      padding: 12px 18px;
      color: var(--gray-800);
      text-align: center;
      border-radius: 22px;
    }
  }

  &:hover {
    transform: translateY(-8px);
    box-shadow: 8px 9px 19px 0px rgb(0 0 0 / 30%);

    .prod-price {
      transform: translateY(0%);
      .price {
        transform: translateY(0%);
      }
    }
    .icon-redeem img:first-child {
      opacity: 0;
    }
  }
}

.btn-primary {
  background-color: var(--main-blue);
  min-width: 200px;
  padding: 12px 18px;
  color: #fff;
  text-align: center;
  border-radius: 22px;
  font-weight: 800;
  font-size: 1.2rem;
  border: none;
  outline: none;
  cursor: pointer;

  &:hover {
    background-color: var(--color-accent);
    color: #fff;
  }
}

@keyframes ani-num {
  0% {
    color: rgb(52, 52, 52);
    transform: scale(1);
  }
  50% {
    transform: scale(1.3);
  }
  to {
    color: rgb(247, 131, 8);
    transform: scale(1);
  }
}

.ani-number {
  animation-name: ani-num;
  animation-duration: 0.5s;
}

// Helpers
.d-none {
  display: none;
}
.d-flex {
  display: flex;
}
.jc-between {
  justify-content: space-between;
}
.jc-center {
  justify-content: center;
}
.mx-auto {
  margin: 0 auto;
}
.mx-2 {
  margin: 0 0.5rem;
}
.ml-1 {
  margin-left: 0.2rem;
}
.mb-4 {
  margin-bottom: 2.4rem;
}
.color-dark-gray {
  color: var(--gray-800);
}
.border-bottom {
  border-bottom: 1px solid var(--gray-400);
}
.img-fluid {
  max-width: 100%;
  height: auto;
}
</style>
