<template>
  <div id="posNav">
    <el-row>
      <el-col :span='7' id="order-list">
        <el-tabs>
          <el-tab-pane label='点餐'>
            <el-table :data="tableData" border stripe style="width: 100%" height="450">
          <el-table-column prop="goodsName" label="商品名称" width="100">
          </el-table-column>
          <el-table-column prop="price" label="价格" width="60">
          </el-table-column>
          <el-table-column prop="count" label="数量" width="60">
          </el-table-column>
          <el-table-column label="操作" fixed='right' >
            <template slot-scope='scope'>
            <el-button  type="text" size="small" @click="delsinglegoods(scope.row)">删除</el-button>
            <el-button  type="text" size="small" @click="addOrderList(scope.row)">增加</el-button>
        </template>
          </el-table-column>
        </el-table>
        <div id='sum'>数量：{{totalCount}}  <br>金额：{{totalMoney}}元</div>
        <el-button type="warning" style="margin-top:10px;">挂单</el-button>
        <el-button type="danger" style="margin-top:10px;" @click="delallgoods()">删除</el-button>
        <el-button type="success" style="margin-top:10px;" @click="bill()">结账</el-button>
          </el-tab-pane>
          <el-tab-pane label='挂单'></el-tab-pane>
          <el-tab-pane label='外卖'></el-tab-pane>
        </el-tabs>
      </el-col>
      <el-col :span='17'>
        <div>
          <div id="rightHeader">热销商品</div>
          <div><ul id="foodcontainer">
            <li v-for="goods in oftenGoods" :key='goods.id' id="oftenGoods" @click='addOrderList(goods)'>
            <span>{{goods.goodsName}}</span>
            <span>￥{{goods.price}}元</span>
            </li></ul>
            </div>
           <el-tabs>
             <el-tab-pane label='汉堡类'>
               <li v-for="goods in type0Goods" :key='goods.id' id="foodseries" v-on:click='addOrderList(goods)'>
                <span class="foodImg"><img :src="goods.goodsImg" width="100%"></span>
                <span>{{goods.goodsName}}</span>
                <span>￥{{goods.price}}元</span>
            </li>
             </el-tab-pane>
             <el-tab-pane label='小吃类'>
               <li v-for="goods in type1Goods" :key='goods.id' id="foodseries" @click='addOrderList(goods)'>
                <span class="foodImg"><img :src="goods.goodsImg" width="100%"></span>
                <span>{{goods.goodsName}}</span>
                <span>￥{{goods.price}}元</span>
            </li>
             </el-tab-pane>
             <el-tab-pane label='饮料类'>
               <li v-for="goods in type2Goods" :key='goods.id' id="foodseries" @click='addOrderList(goods)'>
                <span class="foodImg"><img :src="goods.goodsImg" width="100%"></span>
                <span>{{goods.goodsName}}</span>
                <span>￥{{goods.price}}元</span>
            </li>
             </el-tab-pane>
           </el-tabs>
        </div>
      </el-col>
    </el-row>
  </div>
</template>

<script>
/* eslint-disable */ 
import axios from 'axios'
  export default {
    name: 'pos',
    created:function(){
      axios.get('http://jspang.com/DemoApi/oftenGoods.php')
      .then(response=>{
        this.oftenGoods=response.data;
      })
      .catch(error=>{
        console.log(error);
        alert('错误不能访问');
      })
      axios.get('http://jspang.com/DemoApi/typeGoods.php')
      .then(response=>{
        this.type0Goods=response.data[0];
        this.type1Goods=response.data[1];
        this.type2Goods=response.data[2];
        this.type3Goods=response.data[3];
      })
      .catch(error=>{
        console.log(error);
        alert('错误不能访问');
      })
    },
    mounted: function () { 
      var orderHeight=window.innerHeight;
      document.getElementById("order-list").style.height=orderHeight+'px';
      },
    data() {
      return {
        tableData:[],
        oftenGoods:[],
        type0Goods:[],
        type1Goods:[],
        type2Goods:[],
        totalMoney:0,
        totalCount:0
      }
    },
    methods:{
      addOrderList(goods){
            this.totalCount = 0;
            this.totalMoney = 0;   
            let isHave=false;
            //判断是否这个商品已经存在于订单列表
            for (let i=0; i<this.tableData.length;i++){
                console.log(this.tableData[i].goodsId);
                if(this.tableData[i].goodsId==goods.goodsId){
                    isHave=true; //存在
                }
            }
            //根据isHave的值判断订单列表中是否已经有此商品
            if(isHave){
                //存在就进行数量添加
                 let arr = this.tableData.filter(o =>o.goodsId == goods.goodsId);
                 arr[0].count++;
                 //console.log(arr);
            }else{
                //不存在就推入数组
                let newGoods={goodsId:goods.goodsId,goodsName:goods.goodsName,price:goods.price,count:1};
                 this.tableData.push(newGoods);
            }
            this.getallmoney();          
      },
      delsinglegoods(goods){
        this.tableData=this.tableData.filter(o=>o.goodsId!=goods.goodsId);
        this.getallmoney();
      },
      delallgoods(){
        this.tableData = [];
        this.totalMoney=0;
        this.totalCount=0;
      },
      getallmoney(){
        //汇总数量和金额
        this.totalCount = 0;
        this.totalMoney = 0;
        if(this.tableData){
          this.tableData.forEach((element) => {
                this.totalCount=element.count+this.totalCount;
                this.totalMoney=this.totalMoney+(element.price*element.count);   
            });
        }
      },
      bill(){
        alert('成功结账'+this.totalMoney+'元');
      }
    }
  }
</script>
<style scoped>
#rightHeader{
  border-bottom: 1px solid rgb(226, 226, 207);
  height: 39px;
}
#foodcontainer{
  height: 348px;
  width: 100%;
  
}
#oftenGoods{
  list-style: none;
  border: 1px solid rgb(206, 208, 209);
  height: 30px;
  width: 160px;
  padding: 10px;
  float: left;
  margin: 10px;
  cursor: pointer;
}
#foodseries{
  list-style: none;
  float: left;
  padding: 10px;
  margin: 10px;
  border: 1px solid silver;
  width: 250px;
  cursor: pointer;
}
.foodImg img{
  height: 100px;
  width: 100px;
}
#sum{
  font-size: 0.8em;
  color:snow;
  background-color: gray;
}
</style>