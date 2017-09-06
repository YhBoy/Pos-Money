<template>
	<div class="leftNav" id="nav">
		<ul>
			<li>
				<i class="icon iconfont icon-dianpu"></i>
				<em>店铺</em>
			</li>
			<li>
				<i class="icon iconfont icon-shouyin"></i>
				<em>外卖</em>
			</li>
			<li>
				<i class="icon iconfont icon-gerenzhongxinxia"></i>
				<em>设置</em>
			</li>


		</ul>

	</div>
</template>

<script>
export default{
	name : "leftNav"
}

</script>

<style>

.leftNav{ width:5%;
		  height:100%;
		  background:#004FAC; 
		  float:left;}


.icon{ color:#fff ;
	    font-size:20px;}

.leftNav li{ padding:15px 0;
			 cursor:pointer;  }
.leftNav li em{ font-style:normal; 
				font-size:20px;
				display:block;
				color:#fff;
				margin: 5px 0}

</style>