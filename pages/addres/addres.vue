<template>
	<view class="content">
		<view class="padding-xs margin-xs box">
		  <van-row class="flex justify-between padding-xs margin-xs" v-for="(item, index) in info" :key="index">
		    <van-col span="20" class="title">
		      <view>姓名：{{item.name}}</view>
		      <view>电话：{{item.phone}}</view>
		      <view>地址：{{item.address}}</view>
		    </van-col>
		    <van-col span="4" class="btn">
		      <button class="cu-btn bg-white" @click="reset(item.id)">删除</button>
		    </van-col>
		  </van-row>
		</view>
		<!-- 添加地址 -->
		<view class="flex flex-direction add">
		  <button class="cu-btn bg-blue lg" @click="add()">添加地址</button>
		</view>
	</view>
</template>

<script>
export default {
  data() {
    return {
		info:[]
	}
	},
	onLoad:function() {
		// 地址列表
		var _this = this
		uni.request({
			url:'https://star.qhynice.top/api/address/getaddress',
			method:'GET',
			data:{
				user_id: 73
			},
			success(res) {
				console.log(res)
				_this.info = res.data.res
			}
		})
	},
	methods:{
		// 删除地址
		reset:function(id){
			var that = this
			that.id = id
			console.log(id)
			uni.request({
				url:'https://star.qhynice.top/api/address/del_adds',
				method:'POST',
				data:{
					user_id: 73,
					id: id
				},
				success: (res) => {
					let pages = getCurrentPages();	//获取加载的页面
					let page = pages[pages.length - 1];	//获取当前页面的对象
					page.onLoad()
				}
			})
		},
		add:function(){
			uni.navigateTo({
				url:'../tack/tack'
			})
		}
	}
}
</script>

<style scoped>
.box{
  height: 90vh;
  overflow: auto;
}
.title{
  line-height: 1.5;
  font-size: 30rpx;
}
.add{
  padding: 0;
  bottom: 0;
  position: fixed;
  width: 100%;
}
.btn{
	display: flex;
	align-items: flex-end;
}
</style>
