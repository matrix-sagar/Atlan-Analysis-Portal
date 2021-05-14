<template>
<!--<form v-on:submit="getOrderDetails($event)">-->
  <div class="tableWrapper">
      <div class="searchTable">
        <StocksOverview></StocksOverview>
      </div>
      <div class="orderDetailsDiv">
        <div class="searchDiv">
          <p class="orderDetailsHeader">Enter Order number to get order details</p>
          <input type="number" id="orderID" name="OrderID" placeholder="Ex. 10300" pattern="[0-9]{5}">
          <button class="searchButton" v-on:click="getOrderDetails">Search</button>
        </div>
        <div class="eachOrderDetailRow" v-if="orderDetailForOrderID.searchTriggered === true">
          <p class="orderDetailLabel">Customer name: </p>
          <p class="orderDetailValue">{{orderDetailForOrderID.customerName}}</p>
        </div>
        <div class="eachOrderDetailRow" v-if="orderDetailForOrderID.searchTriggered === true">
          <p class="orderDetailLabel">Sales contact: </p>
          <p class="orderDetailValue">{{orderDetailForOrderID.salesContact}}</p>
        </div>
        <div class="eachOrderDetailRow" v-if="orderDetailForOrderID.searchTriggered === true">
          <p class="orderDetailLabel">Ordered date: </p>
          <p class="orderDetailValue">{{orderDetailForOrderID.dateOrdered}}</p>
        </div>
        <div class="eachOrderDetailRow productNamesDiv" v-if="orderDetailForOrderID.searchTriggered === true">
          <p class="orderDetailLabel">Products pruchased: </p>
          <div class="orderDetailValue productNames">
            <ul >
              <li v-for="product in orderDetailForOrderID.productNames" :key="product.message">{{product}}</li>
            </ul>
          </div>
        </div>
      </div>
  </div>
<!--
</form>-->
</template>


<script>
  import StocksOverview from './StocksOverview';
export default {
    name: 'Orders',
    components : {
      StocksOverview: StocksOverview
  },
  beforeCreate() {
    console.log("Before Create Orders.vue :: " + new Date().getTime());
  },
  mounted() {
    console.log("Mounted - Orders.vue :: " + new Date().getTime());
  },
    data: function(){
        return{     
            completeOrderList: [], 
            employeeDetails: [],
            customerDetails: [],
            orderDetailsList:[],
            productDetails:[],
            orderDetailForOrderID : {
              searchTriggered : false
          }
        }
    },
    methods: {
        getOrderDetails(){
            //event.preventDefault();
            let selectedOrderId=document.getElementById('orderID').value;

            if(!selectedOrderId) {
              alert("Please enter some value and then hit search button!!");
              return;
            }


            fetch("json/orders.json")
            .then(response=>response.json())
            .then((data)=>{
                this.completeOrderList=data;
            })
            .then(()=>{
                return fetch("json/employees.json")
            })
            .then(response=>response.json())
            .then((data)=>{
                this.employeeDetails=data;

            })
            .then(()=>{
                return fetch("json/customers.json")
            })
            .then(response=>response.json())
            .then((data)=>{
                this.customerDetails=data;

            })
            .then(()=>{
                return fetch("json/order_details.json")
            })
            .then(response=>response.json())
            .then((data)=>{
                this.orderDetailsList=data;

            })
            .then(()=>{
                return fetch("json/products.json")
            })
            .then(response=>response.json())
            .then((data)=>{
                this.productDetails=data;

                let currentOrderDetails=this.completeOrderList.find(order=>order.orderID==selectedOrderId);
                if(currentOrderDetails==null) {
                  alert("Couldn't find Order Details with Order ID: "+selectedOrderId+". Please Try again.");
                  this.orderDetailForOrderID.searchTriggered = false;
                  return;
                }
                else{


                    let customerName="";
                    for(let i=0;i<this.customerDetails.length;i++){
                        if(currentOrderDetails.customerID==this.customerDetails[i].customerID)
                        {
                            customerName=this.customerDetails[i].contactName;
                            this.orderDetailForOrderID.customerName = customerName;
                            break;
                        }
                    }  
                    let productNames=[];
                    for(let i=0;i<this.orderDetailsList.length;i++){
                        if(currentOrderDetails.orderID==this.orderDetailsList[i].orderID){
                            for(let j=0;j<this.productDetails.length;j++){
                                if(this.orderDetailsList[i].productID==this.productDetails[j].productID){
                                    productNames.push(this.productDetails[j].productName);

                                }     
                            }
                        }
                    }
                    this.orderDetailForOrderID.productNames = productNames;
                    let orderedFromEmployee="";   
                    for(let i=0;i<this.employeeDetails.length;i++){
                        if(currentOrderDetails.employeeID==this.employeeDetails[i].employeeID)
                        {
                            orderedFromEmployee=this.employeeDetails[i].titleOfCourtesy+" "+this.employeeDetails[i].firstName+" "+this.employeeDetails[i].lastName;
                            break;
                        }
                    }

                    this.orderDetailForOrderID.salesContact = orderedFromEmployee;
                    this.orderDetailForOrderID.searchTriggered = true;
                    let dateOrdered = currentOrderDetails.orderDate;
                    if(!dateOrdered) {
                      dateOrdered = 'NA'
                    }
                    this.orderDetailForOrderID.dateOrdered = dateOrdered;

                }
            })

        }
    }
}
</script>

<style>
  .searchTable {
    width: 60%;
    float: left;
    min-height: 200px;
    border: 1px solid #d1d3d4;
    border-radius: 5px;
    margin: 0 2% 0 0;
    padding: 10px 2%;
  }
  .orderDetailsDiv {
    width: 30%;
    float: left;
    padding: 10px 1%;
    border: 1px solid #d1d3d4;
    border-radius: 5px;
    min-height:200px;
  }
  .tableWrapper {
    margin: 20px 10px 10px;
    width: 100%;
    box-sizing: border-box;
    overflow: hidden;
  }
  .orderDetailsHeader {
    font-size: 14px;
    font-weight: bold;
    margin: 0 0 10px;
    padding: 0 0 5px;
    border-bottom: 1px solid #dedede;
    text-align: center;
  }
  .eachOrderDetailRow {
    overflow: hidden;
    margin: 5px 0 0;
  }
  .orderDetailLabel {
    width: 135px;
    float: left;
    font-size: 14px;
    font-weight: bold;
    margin: 0 5px 0 0;
  }
  .orderDetailValue {
    width: calc(100% - 145px);
    float: left;
    font-size: 14px;
  }
  .searchButton {
    margin: 0 0 0 10px;
  }
  .productNamesDiv {
    background: #efefef;
    padding: 5px;
    border-radius: 5px;
    margin: 15px 0 0;
  }
  .productNamesDiv ul{
    padding: 0;
    margin: 0;
    list-style : none;
  }
  .productNamesDiv ul {
    padding: 0;
    margin: 0;
    list-style: inside decimal;
  }
  .searchDiv {
    margin: 0 0 15px;
  }

</style>