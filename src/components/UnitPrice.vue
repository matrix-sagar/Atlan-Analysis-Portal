<template>
    <div class="unitPriceDiv" id="unitPriceDiv">
        <!--<button class="btn" v-on:click="getUnitPrice"> Get Unit Price </button>-->
        <label for="FilterUnitPrice">Filter by unit price range:</label>
        <select  id="FilterUnitPrice" name="FilterUnitPrice" @change="onChange($event)" class="form-control">
            <option value="all">Everything</option>
            <option value="30">Less than 30</option>
            <option value="50">Less than 50</option>
            <option value="80">Less than 80</option>
            <option value="100">Less than 100</option>
        </select>
        <div id="chart">
            <apexchart type="bar" height="350" :options="chartOptions" :series="series"></apexchart>
        </div>
    </div>
  <!--<button class="btn" v-on:click="stockDetails">Stock </button>-->
</template>

<script>
import VueApexCharts from "vue3-apexcharts";
export default {
    name: 'UnitPrice',
    created: function () {
        this.getUnitPrice();
    },
    components: {
        apexchart: VueApexCharts,
    },
    data: function() {
    return{
      productDetails:[],
      completeProductDetails: [],
        completeOrderDetails : [],
        dataForGraph1:{
        priceForOneUnitList: [],
        productName:[],
      },
        options1: [{
            text: "name1",
            value: "value1"
        },{
            text: "name1",
            value: "value1"
        }],
       series: [{
        name: "",
        data: [],  
    }],
      chartOptions: {
        chart: {
          height: 350,
          type: 'bar',
          zoom: {
            enabled: false
          }
        },
        plotOptions: {
              bar: {
                horizontal: false,
                columnWidth: '55%',
                endingShape: 'rounded'
              },
            },
        dataLabels: {
          enabled: false
        },
        stroke: {
              show: true,
              width: 2,
              colors: ['transparent']
            },
        title: {
          text: 'Price Per Unit',
          align: 'center'
        },
        grid: {
          row: {
            colors: ['#f3f3f3', 'transparent'], // takes an array which will be repeated on columns
            opacity: 0.5
          },
        },
        xaxis: {
          categories: [],
        },
        tooltip: {
        custom: function({ series, seriesIndex, dataPointIndex, w }) {
          return (
        '<div style="padding: 5px" class="arrow_box">' +
        "<span>" +
          w.globals.labels[dataPointIndex] +
        ": " +
          series[seriesIndex][dataPointIndex] +
        "</span>" +
        "</div>"
      );
    }
  }
      },
    }
  },
  methods: {
    getUnitPrice(){
      var priceForOneUnitList=[];
      var productName=[];
      fetch("json/products.json")
      .then(response=>response.json())
      .then((data)=>{
        this.completeProductDetails=data;
        
      })
      .then(()=>{
        return fetch("json/order_details.json")
      })
      .then(response=>response.json())
      .then((data)=>{
          this.completeOrderDetails = data;
      for(let i=0; i<this.completeProductDetails.length;i++){
        let currentProduct=this.completeProductDetails[i];
        let currentProductId=currentProduct.productID;
        let totalOrders=0;
        let filteredOrderedDetails=data.filter(product=>product.productID==currentProductId)
        for(let j=0;j<filteredOrderedDetails.length;j++){
          totalOrders+=filteredOrderedDetails[j].quantity;
        }
        priceForOneUnitList[i]=this.completeProductDetails[i].unitPrice;
        productName[i]=this.completeProductDetails[i].productName+'['+totalOrders+']';

      }
      this.series=[{
        data: priceForOneUnitList,
      }]
      this.chartOptions={
        xaxis:{
          categories: productName,
        }
      }
      })
    },
    onChange(event){
      let selectedValue=event.target.value;
      let filteredListName=[];
      let filteredListUnitPrice=[];
      for(var i=0;i<this.completeProductDetails.length;i++){
          let currentProduct=this.completeProductDetails[i];
          let totalOrders=0;

          let filteredOrderedDetails=this.completeOrderDetails.filter(product=>product.productID==currentProduct.productID)
          for(let j=0;j<filteredOrderedDetails.length;j++){
              totalOrders+=filteredOrderedDetails[j].quantity;
          }

        if(selectedValue==="all"){
          filteredListUnitPrice.push(this.completeProductDetails[i].unitPrice);
           filteredListName.push(this.completeProductDetails[i].productName + "[" + totalOrders + "]");
         }
         else if(this.completeProductDetails[i].unitPrice<=selectedValue){
           filteredListUnitPrice.push(this.completeProductDetails[i].unitPrice);
           filteredListName.push(this.completeProductDetails[i].productName + "[" + totalOrders + "]");
         }

      }
      this.series=[{
        data: filteredListUnitPrice,
      }]
      this.chartOptions={
        xaxis:{
          categories: filteredListName,
        }
      }

    },
    stockDetails(){
      let unitsInStock=[];
      let unitsInOrder=[];
      for(let i=0;i<this.completeProductDetails.length;i++){
        unitsInStock.push(this.completeProductDetails[i].unitsInStock);
        unitsInOrder.push(this.completeProductDetails[i].unitsOnOrder);
      }


    }
  }

}
</script>

<style scoped>

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: left;
  color: #2c3e50;
  margin-top: 60px;
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
</style>
