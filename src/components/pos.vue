<template>
	<div  class="pos">
		<el-row >
			<el-col :span="7" class="pos-order" id="order-list">
				<el-tabs>
				    <el-tab-pane label="点餐" >
				    	<el-table :data="tableData" style="width:100%;">
				    		<el-table-column prop="goodsName" label="名称"></el-table-column>
				    		<el-table-column prop="count" label="量"></el-table-column>
				    		<el-table-column prop="price" label="金额"></el-table-column>
				    		<el-table-column prop="" label="操作">
				    			<template scope="scope">
				    				<el-button type="text" size="samll" @click="delSingGodds(scope.row)">删除</el-button>
				    				<el-button type="text" size="samll" @click="addOrderList(scope.row)">增加</el-button>
				    			</template>
				    		</el-table-column>
				    	</el-table>

				    	<div >

				    		数量:  {{totalCount}}</br>
				    		金额:  {{totalMoney}}

				    	</div>

				    	<div class="a-btn">
				    		<el-button type="warning">挂单</el-button>
						    <el-button type="danger" @click="delAllGood">删除</el-button>
						    <el-button type="success" @click="checkOut">结账</el-button>

				    	</div>
				    </el-tab-pane>
				    <el-tab-pane label="外卖">
				   	 	外卖
				    </el-tab-pane>
				    <el-tab-pane label="挂单" >
				    	挂单
				    </el-tab-pane>
				   
				</el-tabs>

			</el-col>

			<el-col  :span="17">
				<div class="often-goods">
					<div class="title">常用商品</div>
					<div class="often-goods-list">
						<ul>
							<li v-for="goods in oftenGoods" @click="addOrderList(goods)">
								<span>{{goods.goodsName}}</span>
								<span class="o-price">{{goods.price}}</span>
							</li>

						</ul>
					</div>
					<div class="goods-type">
						 <el-tabs>
						    <el-tab-pane label="汉堡" >
						    	<div >
						    		<ul class="cookList ">
						    			<li v-for="goods in type0Goods" @click="addOrderList(goods)">
						    				<span class="foodImg"><img :src="goods.goodsImg" width="100%"></span>
										    <span class="foodName">{{goods.goodsName}}</span>
										    <span class="foodPrice">￥{{goods.price}}元</span>
						    			</li>
						    		</ul>
						    	</div>
						    </el-tab-pane>
						    <el-tab-pane label="小吃" >
						    	<div >
						    		<ul class="cookList ">
						    			<li v-for="goods in type1Goods" @click="addOrderList(goods)">
						    				<span class="foodImg"><img :src="goods.goodsImg" width="100%"></span>
										    <span class="foodName">{{goods.goodsName}}</span>
										    <span class="foodPrice">￥{{goods.price}}元</span>
						    			</li>
						    		</ul>
						    	</div>
						    </el-tab-pane>
						    <el-tab-pane label="饮料">
						    	<div >
						    		<ul class="cookList ">
						    			<li v-for="goods in type2Goods" @click="addOrderList(goods)">
						    				<span class="foodImg"><img :src="goods.goodsImg" width="100%"></span>
										    <span class="foodName">{{goods.goodsName}}</span>
										    <span class="foodPrice">￥{{goods.price}}元</span>
						    			</li>
						    		</ul>
						    	</div>
						    </el-tab-pane>
						    <el-tab-pane label="套餐" >
						    	<div >
						    		<ul class="cookList ">
						    			<li v-for="goods in type3Goods" @click="addOrderList(goods)">
						    				<span class="foodImg"><img :src="goods.goodsImg" width="100%"></span>
										    <span class="foodName">{{goods.goodsName}}</span>
										    <span class="foodPrice">￥{{goods.price}}元</span>
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
import axios from 'axios'
export default {
	name : "pos",
	data (){
		return {
			tableData :[],
	        oftenGoods:[],
	        type0Goods:[],
	        type1Goods:[],
	        type2Goods:[],
	        type3Goods:[],
	        totalMoney:0,
	        totalCount:0

			}
	},
	created(){
		axios.get("http://jspang.com/DemoApi/oftenGoods.php")
			.then( (response)=>{
				this.oftenGoods = response.data
			 })
			.catch( (error) => {
				alert("刷新页面")
		});

		/*2*/

		axios.get("http://jspang.com/DemoApi/typeGoods.php")
			.then( (response)=>{
				this.type0Goods=response.data[0];
				this.type1Goods=response.data[1];
				this.type2Goods=response.data[2];
				this.type3Goods=response.data[3];
			 })
			.catch( (error) => {
				alert("刷新页面")
		})

	},
	mounted () {
		let oHeight=document.body.clientHeight;
    	document.getElementById("order-list").style.height= oHeight + "px";
	},
	methods:{
		addOrderList( goods ){
		this.totalMoney = 0;
		this.totalCount = 0;
			// 商品是否存在 在订单列表中
			let isHave = false ;
			for( let i = 0; i < this.tableData.length; i++ ){
				 if( this.tableData[i].goodsId == goods.goodsId ){
				 	isHave = true ;
				 }
			}
			// 根据判断的值写业务逻辑

			if( isHave ){
				let arr = this.tableData.filter(o=>o.goodsId == goods.goodsId);
				arr[0].count ++;
			}else{
				let nerGoods = {goodsId:goods.goodsId,goodsName:goods.goodsName,price:goods.price,count:1};
				this.tableData.push(nerGoods);
			}


			// 计算金额和价格数量
			this.tableData.forEach( (element) => {
				this.totalCount += element.count;
				this.totalMoney = this.totalMoney + ( element.count * element.price)
			})

		},

		//删除单个商品
		delSingGodds( goods ){
			this.tableData = this.tableData.filter( o=>o.goodsId  != goods.goodsId);
            this.getAll();
		},


		getAll(){
			this.totalCount = 0 ;
			this.totalMoney = 0 ;

			if( this.tableData ){
				this.tableData.forEach( element => {
					this.totalCount += element.count;
					this.totalMoney = this.totalMoney + ( element.count * element.price )
				} )
			}
		},
		delAllGood(){
			this.tableData = [];
			this.totalMoney = 0;
			this.totalCount = 0;
		},

		// 模拟结账
		checkOut(){
			if ( this.totalCount != 0 ){
				 this.totalCount = 0;
				 this.totalMoney = 0;
				 this.tableData = [];
				 alert("结账成功");
			}
		}


	}
}

</script>


<style>

.pos{ float:left;
	  width:95%; }

.pos-order{ background:#f9f9f9; }

.a-btn{ margin-top:10px; }

.title{
   height: 21px;
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


.cookList li{
   list-style: none;
   width:23%;
   border:1px solid #E5E9F2;
   height: auot;
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

.goods-type{ clear:both; }

</style>