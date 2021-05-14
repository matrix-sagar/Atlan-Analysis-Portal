<template>
    <div class="stockTableHeadingWrapper">
        <p class="stockTableHeading">Stock Overview</p>
        <div class="legendWrapper">
            <p class="legendEach">
                <span class="legendIcon legendHighlight"></span>
                <span class="legendText">Stock deficit</span>
            </p>
            <p class="legendEach">
                <span class="legendIcon legendDiscontinued"></span>
                <span class="legendText">Discontinued</span>
            </p>
        </div>
    </div>

    <div class="stockTableWrapper">
        <table>
            <thead>
            <th>Product Id</th>
            <th>Product Name</th>
            <th>Units in Stock</th>
            <th>Units On Order</th>
            </thead>
            <tbody>
            <tr v-for="stock in stocksReview" :key="stock.productID" v-bind:class="{ 'discontinuedClass': stock.discontinued, 'highlightClass': stock.highlight }" >
                <td>{{stock.productID}}</td>
                <td>{{stock.productName}}</td>
                <td>{{stock.unitsInStock}}</td>
                <td>{{stock.unitsOnOrder}}</td>
            </tr>
            </tbody>
        </table>
    </div>

</template>

<script>
export default {
    name: 'StocksOverview',
    created: function () {
        this.getStockDetails();
    },
    beforeCreate() {
        console.log("Before Create StockOverview.vue :: " + new Date().getTime());
    },
    mounted() {
        console.log("Mounted - StockOverview.vue :: " + new Date().getTime());
    },
    data: function(){
        return{
            stocksReview:[],
            completeProductsList:[],
        }
    },
    methods: {
        getStockDetails(){
            fetch("json/products.json")
            .then(response=>response.json())
            .then((data)=>{
                this.completeProductsList=data;

                for(let i=0;i<this.completeProductsList.length;i++){
                    let obj = {};
                    let currentProduct = this.completeProductsList[i];
                    obj.productID = currentProduct.productID;
                    obj.productName = currentProduct.productName;
                    obj.unitsInStock = currentProduct.unitsInStock;
                    obj.unitsOnOrder = currentProduct.unitsOnOrder;
                    obj.discontinued = currentProduct.discontinued;

                    if(currentProduct.unitsInStock < currentProduct.unitsOnOrder){
                        obj.highlight = true;
                    } else {
                        obj.highlight = false;
                    }

                    this.stocksReview.push(obj);
                }
            })
            
        }
    }
}
</script>

<style>
    .searchTable table {
        width: 100%;
        table-layout: fixed;
        border: 1px solid #efefef;
        border-collapse: collapse;
        border-radius: 5px;

    }
    .searchTable table tr {
        border-bottom: 1px solid #efefef;
    }
    .searchTable table tr:first-child {
        border-top: 1px solid #efefef;
    }
    .searchTable table tr:last-child {
        border-bottom: none;
    }
    .searchTable table th, .searchTable table td {
        padding: 5px 0 5px 5px;
    }
    .highlightClass,.legendHighlight {
        background: rgba(223,0,0,0.2);
    }
    .discontinuedClass ,.legendDiscontinued{
        background: #bbb;
    }
    .stockTableHeading {
        font-size: 18px;
        font-weight: bold;
        margin: 15px 0;
        width:200px;
        float: left;
    }
    .stockTableWrapper {
        max-height: 400px;
        overflow: auto;
    }
    .legendWrapper {
        width:calc(100% - 250px);
        float:right;
        margin: 15px 0;
    }
    .stockTableHeadingWrapper {
        overflow: hidden;
    }
    .legendEach {
        display: inline-block;
        margin: 0 20px 0 0;
        float: right;
    }
    .legendEach span {
        display: inline-block;
    }
    .legendIcon {
        width:10px;
        height: 10px;
        margin: 0 5px;
    }

</style>
