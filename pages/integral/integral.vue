<template>
	<view class="content">
		<view class="inter"></view>
		<view class="integral">
			<view>
				<text>我的积分</text>
				<text>{{ jifen }}</text>
			</view>
			<view>
				<text>待审核积分</text>
				<text>{{ daishen }}</text>
			</view>
		</view>
		<!-- 输入框 -->
		<view class="sheet">
			<form>
				<view class="entry">
					<view>姓名</view>
					<input type="text" v-model="username" @blur="name()" placeholder="请输入收款人姓名" placeholder-class="color:#4a4a4a" maxlength="11" />
				</view>
				<view class="entry">
					<view>收款人电话</view>
					<input type="text" v-model="phone" @blur="iphone()" placeholder="请输入真实收款人电话" placeholder-class="color:#4a4a4a" />
				</view>
				<view class="entry">
					<view>提现账户</view>
					<input type="text" v-model="accounts" @blur="account()" placeholder="请输入银行卡号或支付宝账号" placeholder-class="color:#4a4a4a" />
				</view>
				<view class="entry">
					<view>提现金额</view>
					<input type="text" v-model="withdraw" @input="change()" placeholder="请输入提现金额" placeholder-class="color:#4a4a4a" />
				</view>
				<view class="entry">
					<view>实际到账</view>
					<input type="text" v-model="money" disabled="true" placeholder-class="color:#4a4a4a" />
				</view>
				<view class="zhu"><text>注：提现每笔扣除10%手续费</text></view>
			</form>
			<!-- 提交按钮 -->
			<view class="flex solid-bottom padding justify-center">
				<van-button color="linear-gradient(to right, #4bb0ff, #6149f6)" @click="sub()">提交申请</van-button>
			</view>
		</view>
	</view>
</template>

<script>
export default {
	data() {
		return {
			jifen: '',
			daishen: '',
			username: '',
			phone: '',
			accounts: '',
			withdraw: '',
			money: ''
		};
	},
	onShow: function() {
		// 用户信息接口
		var _this = this
		// var url = _this.Global.url
		uni.request({
			url: 'https://exam.qhynice.top/index.php/Api/User/userinfo',
			data: {
				uid: uni.getStorageSync('id')
			},
			header: {
				'content-type': 'application/x-www-form-urlencoded'
			},
			method: 'POST',
			success: res => {
				this.jifen = res.data.userinfo.jifen;
			}
		});
		// 待审核积分
		uni.request({
			url:'https://exam.qhynice.top/index.php/Api/User/pending',
			data: {
				uid: uni.getStorageSync('id')
			},
			header: {
				'content-type': 'application/x-www-form-urlencoded'
			},
			method: 'POST',
			success: res => {
				this.daishen = res.data.data;
			}
		});
	},
	methods: {
		// 判断用户姓名
		name: function() {
			if (this.username == '' || this.username == null) {
				uni.showToast({
					title: '收款人姓名必填',
					duration: 2000,
					icon: 'none'
				});
				return false;
			} else if (!/^[\u4E00-\u9FA5]{2,4}$/.test(this.username)) {
				uni.showToast({
					title: '姓名有误',
					duration: 2000,
					icon: 'none'
				});
				return false;
			}
		},
		// 判断用户手机号
		iphone: function() {
			if (this.phone == '' || this.phone == null) {
				uni.showToast({
					title: '收款人电话不能为空',
					duration: 2000,
					icon: 'none'
				});
			} else if (!/^1[34578]\d{9}$/.test(this.phone)) {
				uni.showToast({
					title: '手机号码有误',
					duration: 2000,
					icon: 'none'
				});
				return false;
			}
		},
		// 判断输入银行账户/支付宝账号
		account: function() {
			if (this.accounts == '' || this.accounts == null) {
				uni.showToast({
					title: '请填写账户',
					duration: 2000,
					icon: 'none'
				});
			} else if (!/^(?:\w+\.?)*\w+@(?:\w+\.)+\w+|\d{9,11}$/.test(this.accounts)) {
				uni.showToast({
					title: '银行卡号/支付宝账号有误',
					duration: 2000,
					icon: 'none'
				});
				return false;
			}
		},
		change: function() {
			this.money = +this.withdraw - this.withdraw * 0.1;
		},
		sub: function() {
			if (this.username == '') {
				uni.showToast({
					title: '请填写账号信息',
					duration: 2000,
					icon: 'none'
				});
				return false;
			} else if (this.phone == '') {
				uni.showToast({
					title: '请填写手机号',
					duration: 2000,
					icon: 'none'
				});
				return false;
			} else if (this.accounts == '') {
				uni.showToast({
					title: '请填写账号信息',
					duration: 2000,
					icon: 'none'
				});
				return false;
			} else if (this.money == '') {
				uni.showToast({
					title: '请输入提现额度',
					duration: 2000,
					icon: 'none'
				});
				return false;
			} else {
				// var that = this
				// var url = this.Global.url
				uni.request({
					url: 'https://exam.qhynice.top/index.php/Api/User/withdraw',
					method: 'POST',
					data: {
						uid: uni.getStorageSync('id'),
						name: this.username,
						tel: this.phone,
						jifen: this.money,
						num: this.withdraw,
						account: this.accounts
					},
					header: {
						'content-type': 'application/x-www-form-urlencoded'
					},
					success: res => {
						uni.redirectTo({
							url: 'integral'
						});
					}
				});
			}
		}
	}
};
</script>

<style scoped>
.inter {
	height: 280rpx;
	background-image: url(../../static/head.jpg);
	background-size: 100%;
}
.integral {
	width: 86%;
	height: 9rem;
	background-image: url(../../static/jifen.jpg);
	background-size: 100%;
	display: flex;
	justify-content: space-around;
	margin: -150rpx 50rpx 0rpx 50rpx;
	align-items: center;
	color: white;
}
.integral view {
	display: flex;
	flex-direction: column;
}
.sheet {
	width: 86%;
	margin-left: 7%;
}
.entry {
	display: flex;
	justify-content: space-between;
	height: 90rpx;
	align-items: center;
	border-bottom: 1px solid #999999;
	font-size: 26rpx;
}
.entry input {
	width: 53%;
}
.zhu {
	display: flex;
	justify-content: flex-end;
	font-size: 26rpx;
}
</style>
