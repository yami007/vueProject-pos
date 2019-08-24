<template>
  <div class="pos">
    <el-row>
      <el-col :span="7" class="pos-order" id="order-list">订单栏</el-col>
      <el-col :span="7">产品栏</el-col>
    </el-row>
  </div>
</template>

<script>
  export default {
    name: 'pos',
    data () {
      return {
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
</style>
