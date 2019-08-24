<template>
  <div class="pos">
    <el-row>
      <el-col :span="7" class="pos-order" id="order-list">
        <el-tabs v-model="activeName" @tab-click="handleClick">
          <el-tab-pane label="点餐" name="first" >
            <el-table :data="tableData" border style="width: 100%">
              <el-table-column prop="goodsName" label="商品名称" ></el-table-column>
              <el-table-column prop="count" label="数量" width="50"></el-table-column>
              <el-table-column prop="price" label="金额" width="70"></el-table-column>
              <el-table-column  label="操作" width="100" fixed="right">
                <template scope="scope">
                  <el-button type="text" size="small">添加</el-button>
                  <el-button type="text" size="small">删除</el-button>
                </template>
              </el-table-column>
            </el-table>
            <div class="div-btn">
              <el-button type="danger">挂单</el-button>
              <el-button type="warning">删除</el-button>
              <el-button type="success">结账</el-button>
            </div>
          </el-tab-pane>
          <el-tab-pane label="挂单" name="second">挂单</el-tab-pane>
          <el-tab-pane label="外卖" name="third">外卖</el-tab-pane>
        </el-tabs>
      </el-col>
      <el-col :span="7">产品栏</el-col>
    </el-row>
  </div>
</template>

<script>
  export default {
    name: 'pos',
    data () {
      return {
        tableData: [{
          goodsName: '可口可乐',
          price: 8,
          count:1
        }, {

          goodsName: '香辣鸡腿堡',
          price: 15,
          count:1
        }, {

          goodsName: '爱心薯条',
          price: 8,
          count:1
        }, {

          goodsName: '甜筒',
          price: 8,
          count:1
        }]
      }
    },
    mounted:function () {
      /*element組件自定义了一些样式，如需改变样式，需进行dom操作*/
      var orderHeight = document.body.clientHeight;
      document.getElementById("order-list").style.height=orderHeight+'px'
    }
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
</style>
