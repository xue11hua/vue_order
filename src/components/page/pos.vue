<template>
  <div class="pos">
    <el-row>
    	<el-col :span="7" class="pos-order" id="order-list">
    		<el-tabs>
    			<el-tab-pane label='点餐'>
    				<el-table :data="tabledata" border style="width:100%">
    					<el-table-column prop="goodsName" label="商品名称"></el-table-column>
    					<el-table-column prop="count" label="数量" width="50"></el-table-column>
    					<el-table-column prop="price" label="金额" width="70"></el-table-column>
    					<el-table-column label="操作" width="100" fixed="right">
    						<template scope="scope">
    							<el-button type="text" size="small" @click="degoods(scope.row)">删除</el-button>
    							<el-button type="text" size="small" @click="addlist(scope.row)">增加</el-button>
    						</template>
    					</el-table-column>
    				</el-table>
    				<div class="tota">
    					<small>数量：</small>{{totacut}}  <small>金额：</small>{{totamaey}}
    				</div>
    				<div class="div-btn">
    					<el-button type="warning" @click="guadangoods()">挂单</el-button>
    					<el-button type="danger" @click="deallgoods()">删除</el-button>
    					<el-button type="success" @click="check()">结账</el-button>
    				</div>
    			</el-tab-pane>
    			<el-tab-pane label='挂单'>
              <el-table :data="guadandata" border style="width:100%">
                <el-table-column prop="goodsName" label="商品名称"></el-table-column>
                <el-table-column prop="count" label="数量" width="50"></el-table-column>
                <el-table-column prop="price" label="金额" width="70"></el-table-column>
              </el-table>
              <div class="div-btn">
               
                <el-button type="success" @click="huifu()">恢复</el-button>
              </div>
          </el-tab-pane>
    			
    		</el-tabs>
    	</el-col>
    	<el-col :span="17">
    		<div class='order-list'>
    		<div class="title">
    			常用商品
    		</div>
    		<div class="order-list-good">
    			<ul>
    				<li v-for="goods in oftgooods" @click="addlist(goods)">
    					<span>{{goods.goodsName}}</span>
    					<span class="o-pr">￥{{goods.price}}</span>
    				</li>
    				
    			</ul>
    		</div>
    		</div>
    		<div class="goods-type">
    			<el-tabs>
    				<el-tab-pane label="汉堡">
    					<div>
    						<ul class="cookList">
    							 <li v-for="goods in type0Goods" @click="addlist(goods)">
							        <span class="foodImg"><img :src="goods.goodsImg" width="100%"></span>
							        <span class="foodName">{{goods.goodsName}}</span>
							        <span class="foodPrice">￥{{goods.price}}</span>
							    </li>
    						</ul>
    					</div>
    				</el-tab-pane>
    				<el-tab-pane label="小食">
    					<div>
    						<ul class="cookList">
    							 <li v-for="goods in type1Goods" @click="addlist(goods)">
							        <span class="foodImg"><img :src="goods.goodsImg" width="100%"></span>
							        <span class="foodName">{{goods.goodsName}}</span>
							        <span class="foodPrice">￥{{goods.price}}</span>
							    </li>
    						</ul>
    					</div>
    				</el-tab-pane>
    				<el-tab-pane label="饮料">
    					<div>
    						<ul class="cookList">
    							 <li v-for="goods in type2Goods" @click="addlist(goods)">
							        <span class="foodImg"><img :src="goods.goodsImg" width="100%"></span>
							        <span class="foodName">{{goods.goodsName}}</span>
							        <span class="foodPrice">￥{{goods.price}}</span>
							    </li>
    						</ul>
    					</div>
    				</el-tab-pane>
    				<el-tab-pane label="套餐">
    					<div>
    						<ul class="cookList">
    							 <li v-for="goods in type3Goods" @click="addlist(goods)">
							        <span class="foodImg"><img :src="goods.goodsImg" width="100%"></span>
							        <span class="foodName">{{goods.goodsName}}</span>
							        <span class="foodPrice">￥{{goods.price}}</span>
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
  data(){
  	return{
  		tabledata:[],
      guadandata:[],
  		oftgooods:[],
        type0Goods:[],
        type1Goods:[],
        type2Goods:[],
        type3Goods:[],
        totamaey:0,
        totacut:0
  	}

  },
  created:function(){
  	axios.get('http://jspang.com/DemoApi/oftenGoods.php')
  	.then(response=>{
  		// console.log(response);
  		this.oftgooods=response.data;
  	})
  	.catch(error=>{
  		console.log(error);
  		alert("错误");
  	})
  	axios.get('http://jspang.com/DemoApi/typeGoods.php')
  	.then(response=>{
  		//console.log(response);
  		this.type0Goods=response.data[0];
  		this.type1Goods=response.data[1];
  		this.type2Goods=response.data[2];
  		this.type3Goods=response.data[3];
  	})
  	.catch(error=>{
  		console.log(error);
  		alert("错误");
  	})
  },
  mounted:function(){
  	var orderheight=document.body.clientHeight;
  	
  	document.getElementById('order-list').style.height=orderheight+'px';
  },
  methods:{
  	addlist(goods){
  		this.totamaey=0;
  		this.totacut=0;
  		//判断商品是否已经存在
  		let ishave=false;
  		for(let i=0;i<this.tabledata.length;i++){
  			if(this.tabledata[i].goodsId==goods.goodsId){
  				ishave=true;
  			}
  		}

  		if(ishave){
  			let arr=this.tabledata.filter(a=>a.goodsId==goods.goodsId);
  			arr[0].count++;
  		}else{
  			let newgoods={goodsId:goods.goodsId,goodsName:goods.goodsName,price:goods.price,count:1}
  			this.tabledata.push(newgoods);
  		}
  		this.getmany()
  		
  	},
  	//删除单个商品
  	degoods(goods){
      console.log(goods);
  		this.tabledata=this.tabledata.filter(o=>o.goodsId !=goods.goodsId);
  		this.getmany()
  	},
  	deallgoods(){
  		this.tabledata=[];
      
  		this.totamaey=0;
  		this.totacut=0;
  	},
  	//结账
  	check(){
  		if(this.totacut!=0){
  			this.tabledata=[];

  			this.totamaey=0;
  			this.totacut=0;
  			this.$message({
  				message:"成功",
  				type:"success"
  			});

  		}else{
  			this.$message.error('空的');
  		}
  	},
  	//汇总
  	getmany(){
  		this.totamaey=0;
  		this.totacut=0;
  		if(this.tabledata){
  			//结算汇总钱和数量
	  			this.tabledata.forEach((element)=>{
	  			this.totacut+=element.count;
	  			this.totamaey=this.totamaey+=(element.price*element.count);
	  		})
  		}

  	},
    //挂单
    guadangoods(){
      this.guadandata=[];
      this.guadandata=this.tabledata;
      this.deallgoods();
    },
    //恢复
    huifu(){
      this.tabledata=[];
      this.tabledata=this.guadandata;
      this.getmany();
      this.guadandata=[];
    }
  }
  
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style slot-scope>
 .pos-order{
 	background-color: #f9fafc;
 	border-right:1px solid blue;
	height: 100%;
 	 }
 	 .div-btn{
 	 	margin-top: 10px;
 	 }
 	 .title{
 	 	height: 20px;
 	 	border-bottom: 1px solid #d3dce6;
 	 	background-color: #f9fafc;
 	 	padding:10px;
 	 	text-align: left;
 	 }
 	 .order-list-good ul li{
 	 	list-style: none;
 	 	float: left;
 	 	border:1px solid blue;
 	 	padding:10px;
 	 	margin:10px;
 	 	background-color: #ffffff;
 	 	cursor: pointer;

 	 }
 	 .o-pr{
 	 	color: #58b7ff;
 	 }
 	 .goods-type{
 	 	clear: both;
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
   .tota{
   	background-color: #fff;
   	padding: 10px;
   	border-bottom: 1px solid #eeeeee;
   }

</style>
