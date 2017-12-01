<template>
  <div class='left-nav'>
      <ul>
          <li>
              <i class="icon iconfont icon-qian"></i>
              <div>收银</div>
          </li>
          <li>
              <i class="icon iconfont icon-dianpu"></i>
              <div>店铺</div>
          </li>
          <li>
              <i class="icon iconfont icon-canyin"></i>
              <div>商品</div>
          </li>
          <li>
              <i class="icon iconfont icon-huiyuanqia"></i>
              <div>会员</div>
          </li>
          <li>
              <i class="icon iconfont icon-qian"></i>
              <div>收银</div>
          </li>
          <li>
              <i class="icon iconfont icon-qian"></i>
              <div>统计</div>
          </li>
      </ul>
  </div>
</template>

<script>
export default{
    name:'leftNav'
}
</script>

<style>
.left-nav{
    height:100%;
    width:5%;
    background-color: #1D8ce0;
    color: #fff;
    font-size: 10px;
    float: left;
}
.iconfont{
     font-size: 24px;
}
.left-nav ul{
    padding: 0;
    margin: 0;
}
.left-nav li{
    list-style: none;
    text-align: center;
    border-bottom: 1px solid #20a0ff;
    padding: 10px;
}
</style>
