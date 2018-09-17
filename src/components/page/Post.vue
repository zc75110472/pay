<template>
    <div class='pos'>
      <el-row>
        <el-col :span='10' class='pos-order' id='pos_order'>
          <el-tabs  v-model="activeName" type="border-card">
            <el-tab-pane label="点餐" name="first">
              <el-table border :data="tableData" ref="multipleTable">
                <el-table-column prop='goodsName' label='商品名称' align='center'></el-table-column>
                <el-table-column prop='count' label='数量' width='50' align='center'></el-table-column>
                <el-table-column prop='price' label='价格' width='50' align='center'></el-table-column>
                <el-table-column label='操作' width='180' fixed="right" align='center'>
                  <template slot-scope="scope">
                    <el-button type="danger" size="mini" @click='handleDel(scope.$index,scope.row)'>删除</el-button>
                    <el-button type="success" size="mini" @click='addTba(scope.row)'>增加</el-button>
                  </template>
                </el-table-column>
              </el-table>
              <div class='collect-btn'>
                <p style='margin-bottom: 10px'>数量：{{zong2}}  总价：￥{{zong}}</p>
                <el-button type="danger" @click='delzong'>删除</el-button>
                <el-button type="success" @click='pay'>结账</el-button>
              </div>
            </el-tab-pane>
            <el-tab-pane label="挂单" name="second">挂单</el-tab-pane>
            <el-tab-pane label="外卖" name="third">外卖</el-tab-pane>
          </el-tabs>
        </el-col>
        <el-col :span='14'>
          <div>
            <h3 class='title'>常用商品</h3>
            <div class='often-goods-list'>
              <ul>
                <li v-for='(item,index) in oftenGoods' :key='index' @click='addTba(item)'>
                  <el-tag type='success' hit>{{item.goodsName}} <span>￥{{item.price}}</span></el-tag>
                </li>
              </ul>
            </div>
          </div>
          <div class='goodstype'>
            <el-tabs v-model="activeName2">
              <el-tab-pane label="汉堡" name="f1">
                <ul class='cookList'>
                  <li v-for='(item,index) in type0Goods' :key='index' @click='addTba(item)'>
                    <span class="foodImg"><img :src="item.goodsImg" width="100%"></span>
                    <span class="foodName">{{item.goodsName}}</span>
                    <span class="foodPrice">￥{{item.price}}元</span>
                  </li>
                </ul>
              </el-tab-pane>
              <el-tab-pane label="小食" name="f2">
                <ul class='cookList'>
                  <li v-for='(item,index) in type1Goods' :key='index' @click='addTba(item)'>
                    <span class="foodImg"><img :src="item.goodsImg" width="100%"></span>
                    <span class="foodName">{{item.goodsName}}</span>
                    <span class="foodPrice">￥{{item.price}}元</span>
                  </li>
                </ul>
              </el-tab-pane>
              <el-tab-pane label="饮料" name="f3">
                <ul class='cookList'>
                  <li v-for='(item,index) in type2Goods' :key='index' @click='addTba(item)'>
                    <span class="foodImg"><img :src="item.goodsImg" width="100%"></span>
                    <span class="foodName">{{item.goodsName}}</span>
                    <span class="foodPrice">￥{{item.price}}元</span>
                  </li>
                </ul>
              </el-tab-pane>
              <el-tab-pane label="套餐" name="f4">
                <ul class='cookList'>
                  <li v-for='(item,index) in type3Goods' :key='index' @click='addTba(item)'>
                    <span class="foodImg"><img :src="item.goodsImg" width="100%"></span>
                    <span class="foodName">{{item.goodsName}}</span>
                    <span class="foodPrice">￥{{item.price}}元</span>
                  </li>
                </ul>
              </el-tab-pane>
            </el-tabs>
          </div>
        </el-col>
      </el-row>
    </div>
</template>

<script>
  import Vue from 'vue'
    export default {
        name: "Post",
        data () {
          return {
            activeName: 'first',
            activeName2: 'f1',
            tableData: [],
            oftenGoods:[],
            type0Goods:[],
            type1Goods:[],
            type2Goods:[],
            type3Goods:[],
            multipleSelection: []
          }
        },
        created () {
          this.getGoods();
          this.getGoosList()
        },
        mounted () {
          let mh = document.body.clientHeight;
          document.querySelector('#pos_order').style.height = mh + 'px';
        },
        computed: {
          // 结算总数
          zong () {
            let z_price = 0;
            this.tableData.forEach(val => {
              z_price += val.count * val.price
            })
            return z_price
          },
          zong2 () {
            let z_num = 0;
            this.tableData.forEach(val => {
              z_num += val.count
            })
            return z_num
          }
        },
        methods: {
          getGoods () {
            this.$http.get('https://www.easy-mock.com/mock/5b8b30dbf032f03c5e71de7f/kuaican/oftenGoods').then(rel => {
              this.oftenGoods = rel.data
            }).catch(rel => {
              this.$message.error('网络错误，不能访问');
            })
          },
          getGoosList () {
            this.$http.get('https://www.easy-mock.com/mock/5b8b30dbf032f03c5e71de7f/kuaican/typeGoods').then(rel => {
              this.type0Goods = rel.data[0];
              this.type1Goods = rel.data[1];
              this.type2Goods = rel.data[2];
              this.type3Goods = rel.data[3];
            }).catch(rel => {
              this.$message.error('网络错误，不能访问');
            })
          },
          // 删除
          handleDel (index, row) {
            // 如果有一个就删除一个
            if (row.count > 1) {
              row.count--
            }else {
              // 如果只有一个就删除该项目
              row.count = 0;
              this.tableData.splice(index,1)
            }
          },
          // 往数组里添加商品
          addTba (item) {
            // 没有就加入数组中，有就加数量
            let flag = false;
            for (let i = 0; i < this.tableData.length; i++) {
              if (this.tableData[i].goodsId === item.goodsId) {
                flag = true
              }
            }
            if (flag) {
              let arr = this.tableData.filter(val => val.goodsId === item.goodsId)
              arr[0].count++;

            }else {
              let newGoods = {
                count: 1,
                goodsId: item.goodsId,
                goodsName: item.goodsName,
                price: item.price
              }
              this.tableData.push(newGoods)
            }
          },
          // 整个删除
          delzong () {
            this.$confirm('删除所有商品, 是否继续?', '提示', {
              confirmButtonText: '确定',
              cancelButtonText: '取消',
              type: 'warning'
            }).then(() => {
              this.tableData = [];
              this.$message({
                type: 'success',
                message: '删除成功!'
              });
            }).catch(() => {
              this.$message({
                type: 'info',
                message: '已取消删除'
              });
            });
          },
          // 结账
          pay () {
            if (this.zong) {
              this.$alert(`您的账单为￥${this.zong}元`, '结账', {
                confirmButtonText: '确定',
                type: 'warning'

              }).then(() => {
                this.$message({
                  type: 'success',
                  message: '支付成功!'
                });
                this.tableData = []
              }).catch(() => {
                this.$message({
                  type: 'error',
                  message: '未完成支付'
                });
              })
              }else {
              this.$alert('请先选择商品哦', '结账', {
                confirmButtonText: '确定',
                type: 'warning'
              });
            }

          }
        }
    }
</script>

<style scoped lang='scss'>
  .pos {
    height: 100%;
  }
  .pos-order {
    background: #f9fafc;
    border-right: 2px solid #c0ccda;
  }
  .collect-btn {
    margin-top: 50px;
    text-align: right;
  }
  .title{
    height: 20px;
    line-height: 20px;
    border-bottom:1px solid #D3DCE6;
    background-color: #F9FAFC;
    padding:10px;
  }
  .often-goods-list ul li{
    float:left;
    margin:6px 5px;
    cursor: pointer;
  }
.goodstype {
  clear: both;
  margin-top: 120px;

}
  .cookList li{
    width:23%;
    border:1px solid #E5E9F2;
    height: auto;
    overflow: hidden;
    background-color:#fff;
    padding: 2px;
    float:left;
    margin: 2px;
    cursor: pointer;
    span{
      display: block;
      float:left;
    }
  }
  .foodImg{
    width: 40%;
  }
  .foodName{
    font-size: 14px;
    padding-left: 10px;
    color:brown;
  }
  .foodPrice{
    font-size: 16px;
    padding-left: 10px;
    padding-top:10px;
  }
</style>
