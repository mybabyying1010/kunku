<template>
	<view class="content">
		<view class="box">
			<view class="userinfo">
				<image :src="photo"></image>
				<text class="userinfo-nickname">{{ name }}</text>
			</view>
			<view class="padding flex flex-direction tijiao">
				<button class="cu-btn bg-grey lg" @click="joinus()">Join US</button>
			</view>
		</view>
	</view>
</template>

<script>
export default {
	data() {
		return {
			userInfo: {},
			hasUserInfo: false,
			canIUse: uni.canIUse('button.open-type.getUserInfo'),
			photo: '',
			name: '',
			types:''
		};
	},
	onLoad: function() {
		// 请求邀请人
		uni.request({
			url: 'https://exam.qhynice.top/index.php/Api/User/userinfo',
			method: 'POST',
			data: {
				uid: 44
			},
			header: {
				'content-type': 'application/x-www-form-urlencoded'
			},
			success: res => {
				console.log(res);
				this.userinfo = res.data.userinfo
				this.photo = res.data.userinfo.photo;
				this.name = res.data.userinfo.name;
				var types = res.data.userinfo.type
				this.types = types
			}
		});
		// 判断当前用户是否存在
		if(!uni.getStorageSync('id')){
		  uni.navigateTo({
		    url: '../login/login'
		  })
		}else{
		  // 获取用户信息
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
		      console.log(res)
		      uni.setStorageSync('type', res.data.userinfo.type)
		    }
		  })
		}
	},
	methods:{
		// 点击加入我们按钮
		joinus:function(){
		  var that = this
		  var type = uni.getStorageSync('type')
		  if (type == 1) {
		    uni.switchTab({
		      url: '../index/index',
		    })
		  } else {
		    if (that.types == 1) {
		      uni.request({
		        url: 'https://exam.qhynice.top/index.php/Api/User/user_edit',
		        method: 'POST',
		        data: {
		          uid: uni.getStorageSync('id'),
		          tel: uni.getStorageSync('phoneNumber'),
		          spread_id: that.userinfo.spread_id,
		          secondary_id: that.userinfo.secondary_id,
		          third_id: that.userinfo.third_id,
		          initial_id: that.userinfo.spread_id
		        },
		        header: {
		          'content-type': 'application/x-www-form-urlencoded'
		        },
		        success: res => {
		          uni.switchTab({
		            url: '../index/index',
		          })
		        }
		      })
		    } else {
		      uni.request({
		        url: 'https://exam.qhynice.top/index.php/Api/User/user_edit',
		        method: 'POST',
		        data: {
		          uid: uni.getStorageSync('id'),
		          tel: uni.getStorageSync('phoneNumber'),
		          spread_id: that.userinfo.spread_id,
		          secondary_id: that.userinfo.secondary_id,
		          third_id: that.userinfo.third_id,
		          initial_id: that.userinfo.initial_id
		        },
		        header: {
		          'content-type': 'application/x-www-form-urlencoded'
		        },
		        success: res => {
		          uni.switchTab({
		            url: '../index/index',
		          })
		        }
		      })
		    }
		  }
		}
	}
};
</script>

<style scoped>
.box {
	/* flex-direction: column; */
	background-image: url('https://i2.tiimg.com/704744/016864a33e290365.jpg');
	background-size: 100%;
	height: 100vh;
	width: 100%;
}
.userinfo {
	display: flex;
	flex-direction: column;
	align-items: center;
	padding-top: 120px;
	height: 45vh;
}
.userinfo image {
	width: 20vw;
	height: 13vh;
	border-radius: 50%;
}
.userinfo-avatar {
	width: 128rpx;
	height: 128rpx;
	margin: 20rpx;
	border-radius: 50%;
}

.userinfo-nickname {
	color: #aaa;
	font-size: 40rpx;
}
.cu-btn.button-hover {
	transform: translate(1rpx, 1rpx);
	opacity: 0.8;
}

.tijiao {
	position: fixed;
	width: 100%;
}
</style>
