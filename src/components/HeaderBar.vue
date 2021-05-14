<template>
    <div class="header">
        <div class="logoDiv">
            <p class="logoTextMain">Atlan</p>
            <p class="logoText">Analysis Portal</p>

        </div>
        <p class="pageLoadDiv">Page loaded in <span>{{timeTakenToLoad}}</span> msecs</p>
        <!--<div class="menuDiv">
            <ul>
                <li><a href="#graph1">Products</a> </li>
                <li><a href="#graph1">Orders</a> </li>
            </ul>
        </div>-->
    </div>
</template>

<script>
export default {
    name: 'Header',
    props:{
        timeTakenToLoad : Number
    }
}
</script>
