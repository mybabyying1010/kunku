<template>
	<view class="content">
		<!-- 轮播 -->
		<view>
			<swiper class="swiper" :indicator-dots="indicatorDots" :autoplay="autoplay">
				<swiper-item v-for="(item, index) in info" :key="index">
					<!-- <image style="width: 100%;height: 100%;" :src="item"></image> -->
					<van-image width="100%" height="100%" fit="contain" :src="item" />
				</swiper-item>
			</swiper>
		</view>
		<!-- 信息 -->
		<view>
			<view class="one">
				<view class="name">{{ pro.name }}</view>
			</view>
			<view class="two">
				<view class="money">￥{{ pro.price_yh }}</view>
				<view class="stock">库存：{{ pro.num }}</view>
			</view>
		</view>
		<!-- 标签页 -->
		<view>
			<van-tabs active="a">
				<van-tab title="图文详情" name="a">
					<rich-text :nodes="pro.content"></rich-text>
				</van-tab>
				<van-tab title="产品参数" name="b" class="parameter">
					<view>
						<view>商品名称：{{ pro.name }}</view>
					</view>
					<view>
						<view>商品编号：{{ pro.addtime }}</view>
					</view>
					<view>
						<view>分类：{{ pro.cat_name }}</view>
					</view>
				</van-tab>
			</van-tabs>
		</view>
		<!-- 购买按钮 -->
		<view><button class="btn" @click="showPopup()">立即购买</button></view>
		<!-- 上拉菜单 -->
		<van-action-sheet :show="show" @close="onClose">
			<view>
				<view class="brief">
					<view class="radius"><image class="img" :src="pro.photo_x"></image></view>
					<view class="xinxi">
						<view class="name">{{ pro.name }}</view>
						<view class="money">￥{{ pro.price_yh }}</view>
						<view class="stock">库存：{{ pro.num }}</view>
					</view>
				</view>
				<view class="stepper">
					<view class="radius numbers">购买数量</view>
					<view class="radius"><van-stepper :value="1" @change="onChange()" /></view>
				</view>
				<view class="shops"><button class="shop" @click="onBuyClicked(pro.id)">立即购买</button></view>
			</view>
		</van-action-sheet>
	</view>
</template>

<script>
export default {
	data() {
		return {
			info: [],
			pro: [],
			show: false
		};
	},
	onLoad() {
		var _this = this;
		// var url = _this.Global.url
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
				console.log(res);
				_this.info = res.data.pro.img_arr;
				_this.pro = res.data.pro;
			}
		});
	},
	methods: {
		showPopup() {
			this.show = true;
		},
		onClose() {
			this.show = false;
		},
		onChange(event) {
			uni.setStorageSync('shu', event.detail);
		},
		onBuyClicked() {
			uni.navigateTo({
				url:'../order/order'
			})
		}
	}
};
</script>

<style>
.swiper {
	height: 13rem;
}
.two {
	display: flex;
	justify-content: space-between;
}
.one,.two{
	font-size: 30rpx;
}
.name {
	font-weight: bold;
}
.money {
	color: red;
}
.stock {
	color: gainsboro;
}
.parameter {
	line-height: 3;
	font-size: 36rpx;
}
.btn {
	bottom: 0;
	position: fixed;
	width: 100%;
	background-color: #1989fa;
	color: #fafafa;
}
.brief {
	display: flex;
	flex-wrap: nowrap;
}
.radius {
	margin-left: 6%;
	font-size: 30rpx;
}
.img {
	width: 6rem;
	height: 6rem;
}
.xinxi {
	font-size: 35rpx;
	margin-top: 60rpx;
}
.stepper {
	display: flex;
	justify-content: space-between;
	width: 96%;
	margin-right: 2%;
}
.shops {
	margin-top: 10rpx;
}
.shop {
	background-color: #1989fa;
	color: #fafafa;
}
</style>
