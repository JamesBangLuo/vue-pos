<template>
 <div class="leftnav">
     <ul>
         <li><i class="icon iconfont icon-dianpu"></i><div>店铺</div></li>
         <li><i class="icon iconfont icon-shangpin"></i><div>商品</div></li>
         <li><i class="icon iconfont icon-tongjibaobiao"></i><div>统计</div></li>
         <li><i class="icon iconfont icon-shouye13"></i><div>收银</div></li>
         <li><i class="icon iconfont icon-shouye2"></i><div>会员</div></li>
     </ul>
 </div>
</template>
<script>
export default {
  name: 'leftnav',
  data () {
    return {}
  }
}
</script>
<style>
.leftnav{
    margin: 0px;
    padding: 30px 0px 0px 0px;
    min-width: 5%;
    float: left;
    color: aliceblue;
    background-color: rgb(255, 115, 0);
    font-size: 15px;
    height: 100%;
}
.leftnav ul{
    margin: 0px;
    padding: 0px;
    height: 600px;
}
.leftnav ul li{
    list-style: none;
    padding: 10px;
    border-bottom: 1px solid rgb(255, 145, 0);
    text-align: center;
}
.iconfont{
    font-size: 25px;
}
</style>
