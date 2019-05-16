<template>
  <div class="Pos">
    <div class="">
       <el-row>
         <el-col :span="7" id="order-list">
             <el-tabs type="border-card">
                <el-tab-pane label="点餐">
                  <el-table :data="tableData" border style="width: 100%" >
                    <el-table-column prop="goodsName" label="商品名称"></el-table-column>
                    <el-table-column prop="price" label="数量"></el-table-column>
                    <el-table-column prop="count" label="金额"></el-table-column>
                    <el-table-column label="操作" fixed="right">
                      <template scope="scope">
                          <el-button type="text" size="small" @click="delOrder(scope.row)">删除</el-button>
                          <el-button type="text" size="small" @click="addOrderList(scope.row)">增加</el-button>
                      </template>
                    </el-table-column>
                  </el-table>
                  <div class="totalNum">
                      <span>数量：{{totalCount}}</span>
                      <span>金额：{{totalMoney}}元</span>
                  </div>
                </el-tab-pane>
                <el-tab-pane label="挂单">
                    
                </el-tab-pane>
                <el-tab-pane label="外卖">

                </el-tab-pane>
                <div class="tab-btn">
                   <el-button type="info">挂单</el-button>
                   <el-button type="warning" @click="delAllOrder()">删除</el-button>
                   <el-button type="success" @click="checkOut()">结账</el-button>
                </div>
             </el-tabs>
         </el-col>
         <el-col :span="17">
            <div class="goods-title">常用商品</div>
            <div class="goods-item">
              <ul>
                 <li v-for="goods in goodsData" @click="addOrderList(goods)" >
                   <span>{{goods.goodsName}}</span>
                   <span class="o-price">￥{{goods.price}}元</span>
                 </li>
              </ul>
            </div>
            <div class="goods-type">
               <el-tabs>
                 <el-tab-pane label="汉堡">
                   <ul class='cookList'>
                      <li v-for="goods in typeData" @click="addOrderList(goods)">
                          <span class="foodImg"><img :src="goods.goodsImg" width="100%"></span>
                          <p class="foodName">{{goods.goodsName}}</p>
                          <p class="foodPrice">￥{{goods.price}}元</p>
                      </li>
                  </ul>
                 </el-tab-pane>
                 <el-tab-pane label="小吃">
                    <ul class='cookList'>
                      <li v-for="goods in type1Data" @click="addOrderList(goods)">
                          <span class="foodImg"><img :src="goods.goodsImg" width="100%"></span>
                          <p class="foodName">{{goods.goodsName}}</p>
                          <p class="foodPrice">￥{{goods.price}}元</p>
                      </li>
                    </ul>
                 </el-tab-pane>
                 <el-tab-pane label="饮料">
                    <ul class='cookList'>
                      <li v-for="goods in type2Data" @click="addOrderList(goods)">
                          <span class="foodImg"><img :src="goods.goodsImg" width="100%"></span>
                          <p class="foodName">{{goods.goodsName}}</p>
                          <p class="foodPrice">￥{{goods.price}}元</p>
                      </li>
                   </ul>
                 </el-tab-pane>
                 <el-tab-pane label="套餐">
                    <ul class='cookList'>
                      <li v-for="goods in type3Data" @click="addOrderList(goods)">
                          <span class="foodImg"><img :src="goods.goodsImg" width="100%"></span>
                          <p class="foodName">{{goods.goodsName}}</p>
                          <p class="foodPrice">￥{{goods.price}}元</p>
                      </li>
                   </ul>
                 </el-tab-pane>
               </el-tabs>
            </div>
         </el-col>
       </el-row>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  name: 'Pos',
  data () {
    return {
      tableData:[],
      goodsData:[],
      typeData:[],
      type1Data:[],
      type2Data:[],
      type3Data:[],
      totalCount:0,
      totalMoney:0
    }
  },
  created:function(){
     axios.get('http://jspang.com/DemoApi/oftenGoods.php')
     .then(response=>{
        //console.log(response);
        this.goodsData=response.data;
     })
     .catch(error=>{
        //console.log(error);
        alert("网络出错，sorry");
     })
     axios.get('http://jspang.com/DemoApi/typeGoods.php')
     .then(response=>{
        //console.log(response)
        this.typeData=response.data[0];
        this.type1Data=response.data[1];
        this.type2Data=response.data[2];
        this.type3Data=response.data[3];
     })
     .catch(error=>{
       alert("网络出错，sorry");
     })

  },
  mounted:function(){
     var orderHeight=document.body.clientHeight;
     document.getElementById('order-list').style.height=orderHeight+'px';
  },
  methods:{
    // 添加订单列表的方法
    addOrderList(goods){
      let isHave=false; // 先定义这个订单列表不存在
      this.totalCount=0; // 清零
      this.totalMoney=0; // 
     //判断是否这个商品已经存在于订单列表
     for(let i=0;i<this.tableData.length;i++){
       // console.log(this.tableData[i].goodsId)
       if(this.tableData[i].goodsId==goods.goodsId){
            isHave=true; //存在
       }
     }
      //根据isHave的值判断订单列表中是否已经有此商品
      if(isHave){
         //存在就进行数量添加
         let arr=this.tableData.filter(a=>a.goodsId==goods.goodsId); // 过滤 申明一个a 
         arr[0].count++;
         console.log(arr);
      }else{
        // 不存在就推入数据
        let newArr={goodsId:goods.goodsId,goodsName:goods.goodsName,price:goods.price,count:1};
        this.tableData.push(newArr);
      }
      this.getAllMoney();
    },
    // 删除
    delOrder(goods){
      this.tableData=this.tableData.filter(o => o.goodsId !=goods.goodsId); 
      this.getAllMoney();
    },
    // 统计总额和数量
    getAllMoney(){
       this.totalCount=0; // 清零
       this.totalMoney=0;
       if(this.tableData){
          this.tableData.forEach((element)=>{
             this.totalCount+=element.count;
             this.totalMoney=this.totalMoney+(element.price*element.count);
          })   
       }
    },
    // 删除所有
    delAllOrder(){
       this.tableData=[];
       this.totalCount=0; 
       this.totalMoney=0;
    },

    // 结账
    checkOut(){
      if(this.tableData!=0){
        this.tableData=[];
        this.totalCount=0; 
        this.totalMoney=0;
        this.$message({
           message: '结账成功，感谢你又为店里出了一份力!',
          type: 'success'
        })
      }else{
          this.$message({
            message:'哎呀，现在还不能结账',
            type:'error'
          })
      }
    }

  }
}
</script>

<style >
body,html,p{padding:0;margin:0;height:100%;}
h1, h2 {font-weight: normal;margin:0;padding:0;}
ul {
  list-style-type: none;
  padding: 0;
  margin:0;
}
a {color: #42b983;}
#order-list{background:#f1f1f1;}
.tab-btn{margin-top:15px;}
.goods-title{text-align:left;padding:10px;font-size:20px;color:#666;border-bottom:1px solid #ccc;}
.goods-item,.cookList{overflow:hidden;}
.goods-item ul li{float:left;margin:10px;padding:10px;background:#fff;box-shadow:1px 1px 1px rgba(0,0,0,.5)}
.o-price{color:#e9565c;}
.goods-type{margin-top:10px;margin-left:10px;}
.cookList li{border:1px solid #E5E9F2;background:#fff;display:block;width:23%;height:auto;overflow:hidden;float:left;margin:10px 10px 10px 0;}
.cookList li span,.cookList li p{display:block;float:left;}
.cookList li p{width:55%;text-align:left;}
.foodImg{width:40%;}
.foodName{color:#e9565c;font-size:18px;padding-left:10px;}
.foodPrice{font-size:16px;padding:10px 0 0 10px;}
.totalNum{margin-top:10px;}
</style>
