<template>
  <div class="pos">
    <el-row>
      <el-col :span="7" class="pos-order" id="order-list">
        <el-tabs>
          <el-tab-pane label="点餐" name="first" >
            <el-table :data="tableData" border style="width: 100%">
              <el-table-column prop="goodsName" label="商品名称" ></el-table-column>
              <el-table-column prop="count" label="数量" width="50"></el-table-column>
              <el-table-column prop="price" label="金额" width="70"></el-table-column>
              <el-table-column label="操作" width="100" fixed="right">
                <template scope="scope">
                  <el-button type="text" size="small" @click="addtableData(scope.row)">添加</el-button>
                  <el-button type="text" size="small" @click="delSingleGoods(scope.row)">删除</el-button>
                </template>
              </el-table-column>
            </el-table>
            <div>总数量：{{totalCount}}  总金额{{totalMoney}}</div>
            <div class="div-btn">
              <el-button type="danger">挂单</el-button>
              <el-button type="warning" @click="delAllGoods()">删除</el-button>
              <el-button type="success" @click="checkout()">结账</el-button>
            </div>
          </el-tab-pane>
          <el-tab-pane label="挂单" name="second">挂单</el-tab-pane>
          <el-tab-pane label="外卖" name="third">外卖</el-tab-pane>
        </el-tabs>
      </el-col>
      <el-col :span="17">
        <div class="often-goods">
          <div class="title">常用商品</div>
          <div class="often-goods-list">
            <ul>
              <li v-for="goods in oftenGoods" @click= "addtableData(goods)">
                <span >{{goods.goodsName}}</span>
                <span class="o-price">￥{{goods.price}}元</span>
              </li>
            </ul>
          </div>
        </div>

        <div class="goods-type">
          <el-tabs>
            <el-tab-pane label="汉堡" >
              <div>
                <ul class='cookList'>
                  <li v-for="goods in type0Goods" @click= "addtableData(goods)">
                    <span class="foodImg"><img :src="goods.goodsImg" width="100%"></span>
                    <span class="foodName">{{goods.goodsName}}</span>
                    <span class="foodPrice">￥{{goods.price}}元</span>
                  </li>
                </ul>
              </div>
            </el-tab-pane>
            <el-tab-pane label="小食" >
              <div>
                <ul class='cookList'>
                  <li v-for="goods in type1Goods" @click= "addtableData(goods)">
                    <span class="foodImg"><img :src="goods.goodsImg" width="100%"></span>
                    <span class="foodName">{{goods.goodsName}}</span>
                    <span class="foodPrice">￥{{goods.price}}元</span>
                  </li>
                </ul>
              </div>
            </el-tab-pane>
            <el-tab-pane label="饮料" >
              <div>
                <ul class='cookList'>
                  <li v-for="goods in type2Goods" @click= "addtableData(goods)">
                    <span class="foodImg"><img :src="goods.goodsImg" width="100%"></span>
                    <span class="foodName">{{goods.goodsName}}</span>
                    <span class="foodPrice">￥{{goods.price}}元</span>
                  </li>
                </ul>
              </div>
            </el-tab-pane>
            <el-tab-pane label="套餐" >
              <div>
                <ul class='cookList'>
                  <li v-for="goods in type3Goods" @click= "addtableData(goods)">
                    <span class="foodImg"><img :src="goods.goodsImg" width="100%"></span>
                    <span class="foodName">{{goods.goodsName}}</span>
                    <span class="foodPrice">￥{{goods.price}}元</span>
                  </li>
                </ul>
              </div>
            </el-tab-pane>
          </el-tabs>
        </div>
      </el-col>
    </el-row>
  </div>
</template>

<script>
  import axios from 'axios';
  export default {
    name: 'pos',
    data () {
      return {
        tableData: [],
        oftenGoods:[
        ],
        type0Goods:[
        ],
        type1Goods:[
        ],
        type2Goods:[
        ],
        type3Goods:[
        ],
        totalCount:0,
        totalMoney:0,
      }
    },
    methods:{
      addtableData(goods){
        var goodsId = goods.goodsId
        var isHave = false;
        for(let i = 0; i<this.tableData.length;i++){
          if(this.tableData[i].goodsId === goodsId){
            this.tableData[i].count=this.tableData[i].count+1;
            isHave = true;
          }
        }
        if(isHave){
        }else {
          goods.count= 1;
          this.tableData.push(goods)
        }
        this.getAllMoney();
      },
      //删除单个商品
      delSingleGoods(goods){
        this.tableData=this.tableData.filter(o => o.goodsId !=goods.goodsId);
        this.getAllMoney();
      },
      //汇总数量和金额
      getAllMoney(){
        this.totalCount=0;
        this.totalMoney=0;
        if(this.tableData){
          this.tableData.forEach((element) => {
            this.totalCount+=element.count;
            this.totalMoney=this.totalMoney+(element.price*element.count);
          });
        }
      },
      //删除所有商品
      delAllGoods() {
        this.tableData = [];
        this.totalCount = 0;
        this.totalMoney = 0;
      },
      // 结账
      checkout() {
        if (this.totalCount!=0) {
          this.tableData = [];
          this.totalCount = 0;
          this.totalMoney = 0;
          this.$message({
            message: '结账成功，感谢你又为店里出了一份力!',
            type: 'success'
          });
        }else{
          this.$message.error('不能空结。老板了解你急切的心情！');
        }

      }
    },
    mounted:function () {
      /*element組件自定义了一些样式，如需改变样式，需进行dom操作*/
      var orderHeight = document.body.clientHeight;
      document.getElementById("order-list").style.height=orderHeight+'px'
    },
    created(){
      axios.get('https://www.easy-mock.com/mock/5b8b30dbf032f03c5e71de7f/kuaican/oftenGoods')
        .then(response=>{
          this.oftenGoods=response.data;
        })
        .catch(error=>{
          console.log(error);
          alert('网络错误，不能访问');
        })
      //读取分类商品列表
      axios.get('https://www.easy-mock.com/mock/5b8b30dbf032f03c5e71de7f/kuaican/typeGoods')
        .then(response=>{
          console.log(response);
          //this.oftenGoods=response.data;
          this.type0Goods=response.data[0];
          this.type1Goods=response.data[1];
          this.type2Goods=response.data[2];
          this.type3Goods=response.data[3];

        })
        .catch(error=>{
          console.log(error);
          alert('网络错误，不能访问');
        })
    },
  }
</script>


<style scoped>
  .pos-order{
    background-color: #F9FAFC;
    border-right: 1px solid #C0CCDA;
  }
  .div-btn{
    text-align: center;
    margin-top: 10px;
  }
  .title{
    height: 20px;
    border-bottom:1px solid #D3DCE6;
    background-color: #F9FAFC;
    padding:10px;
  }
  .often-goods-list ul li{
    list-style: none;
    float:left;
    border:1px solid #E5E9F2;
    padding:10px;
    margin:5px;
    background-color:#fff;
  }
  .o-price{
    color:#58B7FF;
  }
  .goods-type{
    clear: both;
    margin:5px;
  }
  .cookList li{
    list-style: none;
    width:23%;
    border:1px solid #E5E9F2;
    height: auto;
    overflow: hidden;
    background-color:#fff;
    padding: 2px;
    float:left;
    margin: 2px;

  }
  .cookList li span{

    display: block;
    float:left;
  }
  .foodImg{
    width: 40%;
  }
  .foodName{
    font-size: 18px;
    padding-left: 10px;
    color:brown;

  }
  .foodPrice{
    font-size: 16px;
    padding-left: 10px;
    padding-top:10px;
  }
</style>
