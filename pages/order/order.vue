<template>
	<view class="content">
		<view><van-card :num="shu" :price="price_yh" :title="name" :thumb="photo_x" title-class="st" /></view>
		<!-- 添加收货地址 -->
		<view class="flex justify-start place">
			<view>
				<image class="icon" :src="icons"></image>
			</view>
			<!-- 收货地址 -->
			<view class="radius addres site" @click="addres()">
				<view class="flex justify-between">
					<view>收货人：{{people.name}}</view>
					<view>{{people.phone}}</view>
				</view>
				<view>地址：{{ people.address }}</view>
			</view>
		</view>
		<view class="paly">
			<view>
				<view class="pay">
					应共支付：
					<span>￥{{ nums }}</span>
				</view>
			</view>
			<!-- 支付按钮 -->
			<view class="padding flex flex-direction">
				<button class="cu-btn bg-blue lg" @click="payment()">线下支付</button>
			</view>
		</view>
	</view>
</template>

<script>
export default {
	data() {
		return {
			shu: '',
			price_yh: '',
			name: '',
			photo_x: '',
			nums: '',
			icons: '../../static/site.png',
			people: [],
			addid: ''
		};
	},
	onLoad() {
		// 商品信息请求显示
		var _this = this
		// var url = _this.Global.url
		this.shu = uni.getStorageSync('shu');
		uni.request({
			url: 'https://exam.qhynice.top/index.php/Api/Product/index',
			method: 'POST',
			data: {
				pro_id: uni.getStorageSync('ids')
			},
			header: {
				'content-type': 'application/x-www-form-urlencoded'
			},
			success: res => {
				this.price_yh = res.data.pro.price_yh;
				this.name = res.data.pro.name;
				this.photo_x = res.data.pro.photo_x;
				this.nums = +this.shu * this.price_yh + '.00';
			}
		})
	},
	onShow() {
		// 默认地址显示
		var _this = this;
		uni.request({
			url: 'https://star.qhynice.top/api/address/getaddress',
			method: 'GET',
			data: {
				user_id: 73
			},
			success(res) {
				console.log(res)
				for (var i in res.data.res) {
					if (res.data.res[i].is_default == 1) {
						_this.people = res.data.res[i];
						var addid = res.data.res[i].id;
						_this.addid = addid;
						return false;
					}
				}
			}
		})
	},
	methods: {
		// 跳转地址列表页面
		addres() {
			uni.navigateTo({
				url: '../addres/addres'
			})
		},
		// 订单提交
		payment() {
			uni.request({
				url: 'https://star.qhynice.top/api/wxpay/index',
				method: 'POST',
				data: {
					userid: 73,	//用户id
					goodsid: uni.getStorageSync('ids'),	//商品id
					goodsname: this.name,	//商品名称
					username: this.people.name,	//收货人姓名
					address: this.people.address,	//收货地址
					phone: this.people.phone,	//收货人电话
					buy_number_count: this.shu,	//商品数量
					total_price: this.nums	//商品总价
				},
				success: function(res) {
					console.log(res);
					if (res.data.state == 200) {
					  uni.showToast({
					  	title: '订单提交成功',
					  	duration: 2000,
					  	icon: 'none'
					  })
					}
				}
			})
		}
	}
}
</script>

<style scoped>
.place {
	margin-top: 50rpx;
}
.icon {
	width: 2.5rem;
	height: 2.5rem;
}
.site {
	font-size: 13px;
	line-height: 1.5;
}
.addres {
	width: 80vw;
	font-size: 37rpx;
}
.pay {
	font-size: 35rpx;
	margin-bottom: 50rpx;
}
.pay span {
	font-size: 38rpx;
	color: red;
}
.paly {
	display: felx;
	flex-direction: column;
	text-align: center;
	margin-top: 300rpx;
}
</style>
