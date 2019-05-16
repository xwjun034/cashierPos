<template>
  <div class="left-nav">
  	 <ul>
  	 	<li>
  	 	  <i class="icon iconfont icon-shouyin"></i>
  	 	  <p>收银</p>
  	 	</li>
  	 	<li>
  	 	  <i class="icon iconfont icon-shangpin"></i>
  	 	  <p>商品</p>
  	 	</li>
  	 	<li>
  	 	  <i class="icon iconfont icon-dianpu"></i>
  	 	  <p>店铺</p>
  	 	</li>
  	 	<li>
  	 	  <i class="icon iconfont icon-gerenzhongxin"></i>
  	 	  <p>会员</p>
  	 	</li>
  	 	<li>
  	 	  <i class="icon iconfont icon-tongji"></i>
  	 	  <p>统计</p>
  	 	</li>
  	 </ul>
  </div>
</template>
<script>
export default{
	name:'leftNav',
	data(){
	  return {

	  }
	}
}
</script>
<style>
.left-nav{width:5%;height:100%;background:#858585;color:#fff;float:left;}
.left-nav ul li{border-bottom:1px solid rgba(255,255,255,.6);font-size:14px;padding:5px 0;}
.left-nav ul li p{padding-top:5px;}
.iconfont{font-size:25px;}
</style>