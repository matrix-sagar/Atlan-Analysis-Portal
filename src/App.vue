<template>
  <Header  v-bind:timeTakenToLoad="timeTakenToLoad"/>
  <!--<Header title="Check Stock Details"/>-->
  <UnitPrice/>
  <Orders/>
</template>

<script>
import Header from './components/HeaderBar.vue'
import UnitPrice from './components/UnitPrice.vue'
import Orders from './components/Orders'

export default {
  name: 'App',
  created() {
    this.createdTime = new Date().getTime();
    console.log("Before Create App.vue :: " + this.createdTime);
  },
  mounted() {
    this.mountedTime = new Date().getTime();
    this.timeTakenToLoad = this.mountedTime - this.createdTime ;
    console.log("Mounted Create App.vue :: " + this.mountedTime);
    /*document.getElementById('pageLoad').innerText = this.timeTakenToLoad;*/
  },
  data: function() {
    return{
      createdTime : '',
      mountedTime : '',
      timeTakenToLoad : ''
    }
    },
  components: {
    UnitPrice: UnitPrice,   
    Header: Header,  
    Orders :Orders
  }, 
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: left;
  color: #2c3e50;
}
body,p {
  margin:0;
  padding: 0;
}
.btn {
  display: inline-block;
  background: #000;
  color: #fff;
  border: none;
  padding: 10px 20px;
  margin: 5px;
  border-radius: 5px;
  cursor: pointer;
  text-decoration: none;
  font-size: 15px;
  font-family: inherit;
}
.btn:focus {
  outline: none;
}
.btn:active {
  transform: scale(0.98);
}
.btn-block {
  display: block;
  width: 100%;
}

.header {
  padding: 10px 20px 0px;
  border-bottom: 1px solid #D1d3D4;
  box-shadow: 0 2px 4px 0px #efefef;
  width: 100%;
  overflow: hidden;
  margin: 0 0 20px 0;
  position: sticky;
  z-index: 50;
  background: #fff;
  top: 0;
  left:0;
  right: 0;
  box-sizing: border-box;

}
.logoDiv {
  width: 150px;
  float: left;
  text-align: center;
  margin: 0 0 10px;
}
.logoTextMain {
  font-size: 30px;
}
.logoText {
  font-size: 15px;
  margin: -9px 0 0 0;
}
.menuDiv {
  width:600px;
  float:left;
  margin: 2px 0 0;
  padding: 0 0 7px 0;
}
.menuDiv ul {
  margin: 30px 0 0 30px;
  list-style: none;
}
.menuDiv li {
  float: left;
  margin: 0;
}
.menuDiv li a {
  text-decoration: none;
  color : #3399ff;
  padding: 5px 20px;
}
.menuDiv li a:hover {
  border-bottom: 2px solid #3399ff;
}
.unitPriceDiv {
  padding: 10px;
  margin: 20px 10px 0;
  border: 1px solid #d1d3d4;
  border-radius: 5px;
}
.pageLoadDiv {
  width: 500px;
  float: right;
  text-align: right;
}
</style>
