<template>
	<view class="content">
		<!-- 微信头像、昵称 -->
		<view class="box">
			<view class="head">
				<view><van-image round width="3rem" height="3rem" :src="userInfo.avatarUrl" /></view>
				<view class="nickname">{{ userInfo.nickName }}</view>
			</view>
		</view>
		<!-- 导航栏 -->
		<view class="usermotto">
			<van-row gutter="20" class="logo">
				<van-col span="8" v-for="(item, key) in shop" :key="key">
					<navigator class="padding" :url="item.url">
						<image class="icon" :src="item.icon"></image>
						<view class="title">{{ item.title }}</view>
					</navigator>
				</van-col>
			</van-row>
		</view>
	</view>
</template>

<script>
export default {
	data() {
		return {
			userInfo: {},
			shop: [
				{
					icon: '../../static/phone.png',
					title: '绑定手机'
				},
				{
					url: '../integral/integral',
					icon: '../../static/integral.png',
					title: '积分兑换'
				},
				{
					icon: '../../static/order.png',
					title: '我的订单'
				},
				{
					url:'../login/login',
					icon: '../../static/grant.png',
					title: '授权'
				}
			]
		};
	},
	onLoad() {
		uni.getUserInfo({
			provider: 'weixin',
			success: res => {
				// console.log(res);
				this.userInfo = res.userInfo;
			}
		});
	}
};
</script>

<style scoped>
.box {
	display: flex;
	align-items: center;
	justify-content: center;
	height: 25vh;
	background-color: #268dcd;
}
.head {
	display: flex;
	flex-direction: column;
	align-items: center;
}
.nickname {
	color: white;
	font-size: 30upx;
}
.usermotto {
	padding-top: 70upx;
}
.logo {
	text-align: center;
	padding-top: 30upx;
}
.icon {
	width: 1.6rem;
	height: 1.6rem;
}
.title {
	font-size: 30upx;
}
</style>
