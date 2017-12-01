<template>
  <div class="pos">
    <el-row>
      <!-- orderlist -->
      <el-col :span='7' class='orderList' id='order_list'>
        <el-tabs id='tab' type='card'>
          <el-tab-pane  label='点单'>
            <el-table border :data='tableData'style='width:100%'>
              <el-table-column prop='goodsName' label='商品名称'></el-table-column>
              <el-table-column prop='count' label='份' width='50'></el-table-column>
              <el-table-column prop='price' label='价格' width='70'></el-table-column>
              <el-table-column label='操作' fixed='right' width='100'>
                <template slot-scope='scope'>
                  <el-button type='text' size='small'@click = 'deleteOrderList(scope.row)'>删除</el-button>
                  <el-button type='text' size='small' @click='addOrderList(scope.row)'>增加</el-button>
                </template>
              </el-table-column>
            </el-table>
            <div class='total'>
              <span class='count-margin' ><small>数量:</small>{{newT}}</span>
              <span class='count-margin'><small>金额:</small>{{newPrice}}元</span>
            </div>
            <div class='btns'>
              <el-button type='warning' >挂单</el-button>
              <el-button type='danger' @click='delAllOrderList'>删除</el-button>
              <el-button type='success'@click='finallyOrderList'>结账</el-button>
            </div>
          </el-tab-pane>
          <el-tab-pane label='挂单'>
            <el-table></el-table>
          </el-tab-pane>
          <el-tab-pane label='外卖'>
            <el-table></el-table>
          </el-tab-pane>
        </el-tabs>
      </el-col>
      <!-- goodslist -->
      <el-col :span='17'>
        <div class='oftenGoods'>
          <div class='title'>常用商品</div>
          <div class='oftenGoodsList'>
            <ul>
              <li v-for="goods in oftenGoods" @click='addOrderList(goods)'>
                <span>{{goods.goodsName}}</span>
                <span class='priceColor'>￥{{goods.price}}</span>
              </li>
            </ul>
          </div>
          <div class='cookList'>
            <el-tabs type='card'>
              <el-tab-pane label="主食">
                <div>
                  <ul>
                    <li v-for="goods in type0Goods" @click='addOrderList(goods)'>
                      <span class="foodImg"><img :src="goods.goodsImg" width="100%"></span>
                      <span class="foodName">{{goods.goodsName}}</span>
                      <p class="foodPrice">&nbsp;&nbsp;￥{{goods.price}}元</p>
                    </li>
                  </ul>
                </div>
              </el-tab-pane>
              <el-tab-pane label="小食">
                <div>
                  <ul>
                    <li v-for="goods in type1Goods" @click='addOrderList(goods)'>
                      <span class="foodImg"><img :src="goods.goodsImg" width="100%"></span>
                      <span class="foodName">{{goods.goodsName}}</span>
                      <span class="foodPrice">&nbsp;&nbsp;￥{{goods.price}}元</span>
                    </li>
                  </ul>
                </div>
              </el-tab-pane>
              <el-tab-pane label="饮料">
                <div>
                  <ul>
                    <li v-for="goods in type2Goods" @click='addOrderList(goods)'>
                      <span class="foodImg"><img :src="goods.goodsImg" width="100%"></span>
                      <span class="foodName">{{goods.goodsName}}</span>
                      <span class="foodPrice">&nbsp;&nbsp;￥{{goods.price}}元</span>
                    </li>
                  </ul>
                </div>
              </el-tab-pane>
              <el-tab-pane label="套餐">
                <div>
                  <ul>
                    <li v-for="goods in type3Goods" @click='addOrderList(goods)'>
                      <span class="foodImg"><img :src="goods.goodsImg" width="100%"></span>
                      <span class="foodName">{{goods.goodsName}}</span>
                      <span class="foodPrice">&nbsp;&nbsp;￥{{goods.price}}元</span>
                    </li>
                  </ul>
                </div>
              </el-tab-pane>
            </el-tabs>
          </div>
        </div>
      </el-col>
    </el-row>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "pos",
  created: function() {
    axios
      .get("http://jspang.com/DemoApi/oftenGoods.php")
      .then(response => {
        this.oftenGoods = response.data;
      })
      .catch(error => {
        console.log("网络出错");
      });
    axios.get("http://jspang.com/DemoApi/typeGoods.php").then(response => {
      this.type0Goods = response.data[0];
      this.type1Goods = response.data[1];
      this.type2Goods = response.data[2];
      this.type3Goods = response.data[3];
    });
  },
  mounted: function() {
    var bodyHeight = document.body.clientHeight;
    document.getElementById("order_list").style.height = bodyHeight + "px";
  },
  data() {
    return {
      tableData: [],
      oftenGoods: [],
      type0Goods: [],
      type1Goods: [],
      type2Goods: [],
      type3Goods: [],
      totalCount: 0,
      totalPrice: 0
    };
  },
  methods: {
    addOrderList: function(goods) {
      this.totalCount = 0;
      //判断列表中是否有此商品
      let isHave = false;
      //循环列表
      for (var i = 0; i < this.tableData.length; i++) {
        if (this.tableData[i].goodsId == goods.goodsId) {
          isHave = true;
        }
      }
      //存在:数量增加
      if (isHave) {
        let arr = this.tableData.filter(a => a.goodsId == goods.goodsId);
        arr[0].count++;
      } else {
        //不存在:将商品信息添加进tabledata数组
        let newGoods = {
          goodsId: goods.goodsId,
          goodsName: goods.goodsName,
          count: 1,
          price: goods.price
        };
        this.tableData.push(newGoods);
      }
    },
    deleteOrderList: function(goods) {
      this.tableData = this.tableData.filter(a => a.goodsId != goods.goodsId);
    },

    delAllOrderList: function(goods) {
      if (this.tableData.length != 0) {
        this.tableData = [];
        this.totalCount = 0;
        this.money = 0;
      } else {
        this.$message.error("暂时没有可以删除的哦~");
      }
    },
    finallyOrderList: function() {
      if (this.totalCount != 0) {
        this.$options.methods.delAllOrderList.bind(this)();
        this.$message({
          message: "结账成功！",
          type: "success"
        });
      } else {
        this.$message.error("还没点单哦，不要心急~");
      }
    },
  },
  computed: {
    //计算总数量
    newT: function() {
      this.totalCount = 0;
      for (var i = 0; i < this.tableData.length; i++) {
        for (var i = 0; i < this.tableData.length; i++) {
          this.totalCount += this.tableData[i].count;
        }
        return this.totalCount;
      }
    },
    //计算总金额
    newPrice: function() {
      var money = 0;
      for (var i = 0; i < this.tableData.length; i++) {
        for (var i = 0; i < this.tableData.length; i++) {
          money = money + this.tableData[i].count * this.tableData[i].price;
        }
        return money;
      }
    },
    
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.orderList {
  background-color: #f9fafc;
  border-right: 1px solid #c0ccda;
}
.btns {
  margin-top: 10px;
}
.title {
  background-color: #f9fafc;
  text-align: left;
  padding: 10px;
  height: 20px;
  border-bottom: 1px solid #d3dce6;
}
.oftenGoodsList ul li {
  list-style: none;
  padding: 10px;
  margin: 10px;
  background-color: #fff;
  border: 1px solid #e5e9f2;
  float: left;
  cursor: pointer;
}
.priceColor {
  color: #58b7ff;
}
.cookList {
  clear: both;
}
.cookList li {
  list-style: none;
  width: 30%;
  border: 1px solid #e5e9f2;
  height: auto;
  overflow: hidden;
  background-color: #fff;
  padding: 2px;
  float: left;
  margin: 2px;
  cursor: pointer;
}
.cookList li span {
  display: block;
  float: left;
}
.foodImg {
  width: 30%;
}
.foodName {
  font-size: 16px;
  padding-left: 10px;
  color: brown;
}
.foodPrice {
  font-size: 16px;
  padding-top: 10px;
  text-align: left;
}
.total {
  text-align: right;
}
.count-margin {
  margin-right: 20px;
}
</style>
