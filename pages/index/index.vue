<template>
	<view class="content">
		<!-- 轮播 -->
		<view>
			<view class="uni-padding-wrap">
				<view class="page-section swiper">
					<view class="page-section-spacing">
						<swiper class="swiper" :indicator-dots="indicatorDots" :autoplay="autoplay">
							<swiper-item v-for="(item, index) in info" :key="index">
								<image style="width: 100%;height: 100%;" :src="item.photo"></image>
							</swiper-item>
						</swiper>
					</view>
				</view>
			</view>
		</view>
		<!-- 商品列表 -->
		<view class="wares">
			<view class="show">
				<view class="single" v-for="(value, key) in shop" :key="key" @click="detail(value.id)">
					<view class="photo">
						<image style="width: 100%;height: 300rpx;" :src="value.photo_x"></image>
						<view class="van-ellipsis name">{{ value.name }}</view>
						<view class="amount">
							<view class="money">￥{{ value.price_yh }}</view>
							<view class="dele">{{ value.price }}</view>
							<view class="sales">销量: {{ value.shiyong }}</view>
						</view>
					</view>
				</view>
			</view>
			<!-- 查看更多 -->
			<view class="see">
				<navigator url="" class="bto">
					<button class="btn" open-type="getPhoneNumber" @getphonenumber="getPhoneNumber">
						获取电话号码
					</button>
				</navigator>
			</view>
		</view>
	</view>
</template>

<script>
export default {
	data() {
		return {
			background: ['color1', 'color2', 'color3'],
			indicatorDots: true,
			autoplay: true,
			info: [],
			shop: []
		};
	},
	onLoad() {
		var _this = this;
		// var url = _this.Global.url
		// console.log(url)
		
		uni.request({
			url: 'https://exam.qhynice.top/index.php/Api/Index/index',
			method: 'POST',
			header: {
				'content-type': 'application/x-www-form-urlencoded'
			},
			success: res => {
				console.log(res);
				_this.info = res.data.ggtop;
				_this.shop = res.data.prolist;
			}
		});
		// uni.navigateTo({
		// 	url: '../bindleader/bindleader'
		// });
	},
	methods: {
		changeIndicatorDots(e) {
			this.indicatorDots = !this.indicatorDots;
		},
		changeAutoplay(e) {
			this.autoplay = !this.autoplay;
		},
		// 进入商品详情页
		detail: function(id) {
			var uid = uni.getStorageSync('id')
			console.log(uid)
			if (!uni.getStorageSync('id')) {
				uni.showModal({
					title: '温馨提示',
					content: '请进行授权',
					success: function(res) {
						if (res.confirm) {
							uni.navigateTo({
								url:'../login/login'
							})
						} else if (res.cancel) {
							uni.switchTab({
								url:'../index/index'
							})
						}
					}
				});
			}else{
				uni.setStorageSync('ids', id);
				uni.navigateTo({
					url: '../details/details'
				});
			}
		},
		// 解密手机号（通过想后台获取的session_key解密手机号）
		getPhoneNumber: function(e) {
			console.log(e);
			console.log(e.detail.errMsg == 'getPhoneNumber:ok');
			if (e.detail.errMsg == 'getPhoneNumber:ok') {
				uni.request({
					url: 'https://exam.qhynice.top/index.php/Api/User/deciphering',
					method: 'GET',
					data: {
						sessionID: uni.getStorageSync('session_key'),
						encryptedDataStr: e.detail.encryptedData,
						iv: e.detail.iv
					},
					header: {
						'content-type': 'application/x-www-form-urlencoded'
					},
					success: res => {
						console.log(res.data.phoneNumber);
					}
				});
			} else {
				uni.showToast({
					title: '获取手机号失败',
					duration: 2000,
					icon: 'none'
				});
			}
		}
	}
};
</script>

<style scoped>
.swiper {
	height: 10rem;
}
.wares {
	width: 90%;
	margin-left: 5%;
	margin-top: 0.3125rem;
}
.show {
	display: flex;
	flex-wrap: wrap;
	justify-content: space-between;
}
.single {
	width: 48%;
	border: 0.0625rem solid #afafaf;
	margin-top: 0.625rem;
	margin-bottom: 0.625rem;
}
.photo {
	margin-top: 0.3125rem;
	width: 94%;
	margin-left: 3%;
}
.amount {
	display: flex;
	flex-wrap: wrap;
	justify-content: space-between;
	padding-top: 30rpx;
	line-height: 2;
}
.name {
	font-size: 0.9375rem;
	color: gray;
}
.money {
	color: #ed1c24;
	font-size: 30rpx;
}
.sales {
	font-size: 0.875rem;
	color: gray;
}
.dele {
	text-decoration: line-through;
	font-size: 25rpx;
}
.see {
	margin-bottom: 20rpx;
}
.btn {
	border: 1px solid gray;
}
</style>
