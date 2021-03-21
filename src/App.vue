<template>
  <div id="app">
    <header>
      <div class="container">
        <div class="row jc-between">
          <img src="@/assets/aerolab-logo.svg" alt="Aerolab">
          <div class="user">
            <div class="user-name color-dark-gray"> Jonh Kite </div>
            <div class="user-points color-dark-gray">
              1000
              <img class="icon icon-coin ml-1 " src="@/assets/icons/coin.svg" alt="coin">
            </div>
          </div>
        </div>
      </div>
    </header>
      <section class="hero">
        <div class="container">
          <h1>Electronics</h1>
        </div>
      </section>
      <main>
        <section class="filters">
          <div class="container">
            <div class="row border-bottom">
              <div class="total-prod color-dark-gray"> 16 of 32  <span class="d-none d-md-block" > products </span> </div>
              <div class="sort-by">
                <span>Sort by:</span>
                <div class="pill active"> Most recent </div>
                <div class="pill"> Lowest price </div>
              </div>
            </div>
          </div>
          <section class="products" >
            <div class="container">
              <div class="prod">
                <div class="prod-status">
                  <div class="icon-redeem"> 
                    <img src="@/assets/icons/buy-blue.svg" alt="redeem"> 
                    <img src="@/assets/icons/buy-white.svg" alt="redeem"> 
                  </div>
                </div>
                <img class="img-fluid prod-img" src="@/assets/product-pics/iPhone8-x1.png" alt="">
                <div class="prod-info">
                  <div class="prod-cat"> Phones </div>
                  <div class="prod-title color-dark-gray"> iPhone 8 </div>
                </div>
                <div class="prod-price">
                    <div class="price">12.000 
                      <img class="icon icon-coin ml-1 " src="@/assets/icons/coin.svg" alt="coin"> 
                    </div>
                    <a class="btn-redeem"> Redeem now</a>
                </div>
              </div>
              <div class="prod">
                <div class="prod-status">
                  <!-- <div class="icon-buy"> <img src="@/assets/icons/buy-blue.svg" alt="redeem"> </div> -->
                  <div class="pill pill-prod">You need 8000
                    <img class="icon icon-coin ml-1 " src="@/assets/icons/coin.svg" alt="coin">
                  </div>
                </div>
                <img class="img-fluid prod-img" src="@/assets/product-pics/iPhone8-x1.png" alt="">
                <div class="prod-info">
                  <div class="prod-cat"> Phones </div>
                  <div class="prod-title color-dark-gray"> iPhone 8 </div>
                </div>
                <div class="prod-price">
                    <div class="price">12.000 
                      <img class="icon icon-coin ml-1 " src="@/assets/icons/coin.svg" alt="coin"> 
                    </div>
                    <a class="btn-redeem"> Redeem now</a>
                </div>
              </div>
            </div>
          </section>
        </section>
      </main>
  </div>
</template>

<style lang="scss">

//Variables
:root {


  --main-blue-rgb:  10,212,250;
  --gray-800-rgb :  97,97,97;
  --main-blue:    #0ad4fa;
  --gray-800:     #616161;
  --gray-500:     #bbbbbb;
  --gray-400:     #d9d9d9;
  --gray-300:     #ededed;
}

//  base
body { 
  margin:0;
  color:#a3a3a3;
  background-color: #f9f9f9;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}


.container { 
  max-width: 1440px;
  padding: 0 16px;
}
.row { display: flex; }
.jc-between { justify-content: space-between; }

header{
  //height: 80px;
  padding: 8px 0;
}

// user
.user {
  display: flex;
  align-items: center;
  justify-content: flex-end;
  min-width: 180px;
  font-size: 1rem;

  &-points{
    display: flex;
    align-items: center;
    margin: 4px 0 4px 16px;
    padding: 4px 12px;
    border-radius: 18px;
    background-color: var(--gray-300);
  }
}
.hero {
  background-image: url('assets/header-x1.png');
  background-size: cover;
  height: 16vh;
  display: flex;
  align-items: flex-end;

  h1{ 
    color: #fff;
    font-weight: 900;
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
    margin-left: 1rem;
    padding-left: 1rem;
    border-left:1px solid var(--gray-500);
  }
}

.pill {
  display: flex;
  align-items: center;
  background-color: var(--gray-300);
  border-radius: 14px;
  padding: 8px 16px;
  font-size: .8rem;
  font-weight: 800;
  margin: 0 4px ;

  &.active {
    background-color: var(--main-blue);
    color: #fff;
  }

  &-prod {
    background-color: rgba(var(--gray-800-rgb), .6);
    color: #fff;
    border-radius: 22px;

    .icon-coin {
      width: 25px;
      height: 25px;
    }
  }
}

.prod {
  display: inline-flex;
  flex-direction: column;
  justify-content: space-between;
  position: relative;
  overflow: hidden;
  background-color: #fff;
  box-shadow:2px 2px 6px 0 rgba(0,0,0,.2);
  padding: 18px;
  margin: 24px;
  width: 280px;
  height: 280px;
  transition: .15s;
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
      transition: .350s;
    }
  }

  &-img {
    margin-bottom: 1rem;
    border-bottom: 1px solid var(--gray-400);
  }

  &-price {
    position: absolute;
    top:0;
    bottom: 0;
    left: 0;
    right: 0;
    background-image:linear-gradient(-180deg, rgb(10 212 250 / 76%) 0%, rgb(37 187 241 / 76%) 100%);
    transition: .350s;
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
      transition: .650s ;
    }
    .btn-redeem {
      background-color: #fff;
      min-width:200px;
      padding: 12px 18px;
      color: var(--gray-800);
      text-align: center;
      border-radius: 22px;
    }
  }

  &:hover {
    transform: translateY(-4px);
    box-shadow:8px 9px 19px 0px rgb(0 0 0 / 30%);

    .prod-price {
      transform: translateY(0%);
      .price {
        transform: translateY(0%);
      }
    }
    .icon-redeem img:first-child{
      opacity: 0;
    }
  }

}

// Helpers
.d-none { display: none; }
.mx-2 { margin:0 0.5rem; }
.ml-1 { margin-left:0.2rem; }
.color-dark-gray { color: var(--gray-800); }
.border-bottom { border-bottom: 1px solid var(--gray-500);}
.img-fluid {    
  max-width: 100%;
  height: auto;
}

</style>
