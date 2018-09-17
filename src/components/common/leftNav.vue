<template>
    <div class='leftnav'>
      <ul>
        <li>
          <i class='iconfont icon-goumai'></i>
          <p>收银</p>
        </li>
        <li>
          <i class='iconfont icon-dianpu'></i>
          <p>店铺</p>
        </li>
        <li>
          <i class='iconfont icon-hanbao'></i>
          <p>商品</p>
        </li>
        <li>
          <i class='iconfont icon-huiyuanqia'></i>
          <p>会员</p>
        </li>
        <li>
          <i class='iconfont icon-quanbudingdan'></i>
          <p>结算</p>
        </li>
        <li>
          <i class='iconfont icon-gongnengjianyi'></i>
          <p>设置</p>
        </li>
      </ul>
    </div>
</template>

<script>
    export default {
        name: "leftNav"
    }
</script>

<style scoped lang='scss'>
  .leftnav {
    float: left;
    width: 60px;
    color: #fff;
    font-size: 12px;
    height: 100%;
    background: #1d8ce0;
    ul {
      padding-top: 20px;
      li {
        padding: 10px;
        text-align: center;
        border-bottom: 1px solid #20a0ff;
        cursor: pointer;
      }
    }
    .iconfont {
      font-size: 36px;
    }
  }

</style>
