<template>
	<view class="container">
		<!-- 上方显示时间 显示wifi的通栏 -->
		<view class="status_bar">
			<view class="top_view"></view>
		</view>
		<view class="nav">
			<view class="nav-left">
				<image src="../../static/img/dt_store_back.png" @click="back"></image>
			</view>
			<view class="nav-title">
				<image src="../../static/img/dun.png"></image>辅导猫
			</view>
		</view>
		<view class="main">
			<view class="checkSuccessView" v-if="checkViewFlag">
				<view class="main-top-state-fangwei">
					<view class="main-top-state-fangwei-animation"></view>
					<view class="main-top-state-fangwei-animation"></view>
					<view class="main-top-state-fangwei-animation"></view>
					<view class="main-top-state-fangwei-animation"></view>
					<view class="main-top-state-fangwei-animation"></view>
					<view class="main-top-state-fangwei-animation"></view>
					<view class="main-top-state-fangwei-animation"></view>
					<view class="main-top-state-fangwei-animation"></view>
					<view class="main-top-state-fangwei-animation"></view>
					<view class="main-top-state-fangwei-animation"></view>
					<view class="main-top-state-fangwei-animation"></view>
					<view class="main-top-state-fangwei-animation"></view>
					<view class="main-top-state-fangwei-animation"></view>
					<view class="main-top-state-fangwei-animation"></view>
					<view class="main-top-state-fangwei-animation"></view>
				</view>
				<view class="main-top-state-fangwei-text">
					<view>动态防伪条</view>
				</view>
				<view class="main-top-state-date">
					<view>当前时间:{{currDate}}</view>
				</view>
				<view class="checkSuccessView-success">
					<image src="../../static/img/done.png"></image>
					<text>签到成功</text>
				</view>
				<ul class="checkSuccessView-ul">
					<li>姓名：{{studentInfo.name}}</li>
					<li>学号：{{studentInfo.stuId}}</li>
					<li>性别：{{studentInfo.sex}}</li>
					<li>民族：{{studentInfo.nation}}</li>
					<li>年级：{{studentInfo.year}}</li>
					<li>学院：{{studentInfo.system}}</li>
					<li>专业：{{studentInfo.subject}}</li>
					<li>班级：{{studentInfo.classNum}}</li>
					<li>宿舍：{{studentInfo.dorm}}</li>
				</ul>
				<image src="../../static/img/exit.png" class="exit" @click="exit"></image>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				currDate: '',
				studentInfo:null,
				checkViewFlag:true,
			}
		},
		onShow() {
			var that = this;
			this.studentInfo = uni.getStorageSync("studentInfo");
			this.refreshCurrDate();
			setInterval(function() {
				that.refreshCurrDate();
			}, 1000)
		},
		methods: {
			//刷新当前时间
			refreshCurrDate: function() {
				var date = new Date();
				this.currDate = date.getFullYear() + "-" +
					(Number(date.getMonth() + 1).toString() <= 1 ? "0" + Number(date.getMonth() + 1) : Number(date.getMonth() + 1)) +
					"-" +
					(date.getDate().toString().length <= 1 ? "0" + date.getDate() : date.getDate()) + " " +
					(date.getHours().toString().length <= 1 ? "0" + date.getHours() : date.getHours()) + ":" +
					(date.getMinutes().toString().length <= 1 ? "0" + date.getMinutes() : date.getMinutes()) + ":" +
					(date.getSeconds().toString().length <= 1 ? "0" + date.getSeconds() : date.getSeconds());
			},
			
			//关闭签到成功窗
			exit:function(){
				this.checkViewFlag = false;
			},
			
			//返回上一页
			back:function(){
				uni.navigateBack({
					delta:1
				})
			}
		}
	}
</script>

<style>
	page {
		background-color: #7F7F7F;
	}

	.container {
		padding: 20px;
		padding-left: 0px;
		padding-right: 0px;
		font-size: 22rpx;
		line-height: 24px;
		height: 100%;
	}

	//上方手机状态栏 通栏
	.status_bar {
		height: 0px;
		width: 100%;
		background-color: #545454;
	}

	.top_view {
		height: var(--status-bar-height);
		width: 100%;
		position: fixed;
		background-color: white;
		top: 0;
		z-index: 999;
	}

	//导航
	.nav {
		position: fixed;
		width: 100%;
		height: 60px;
		background: white;
		border-bottom: 1px solid #efefef;
		z-index: 9999;
	}

	.nav-left {
		padding-top: 24px;
		margin-left: 28rpx;
	}

	.nav-left image {
		width: 20px;
		height: 20px;
	}

	.nav-title {
		font-size: 36rpx;
		position: absolute;
		left: calc(50% - 78rpx);
		top: 16rpx;
	}

	.nav-title image {
		width: 50rpx;
		height: 52rpx;
		position: relative;
		top: 14rpx;
		left: -4rpx;
	}

	.main {
		position: relative;
		top: 60px;
	}

	.checkSuccessView {
		width: 80%;
		height: 366px;
		border-radius: 10px;
		background-color: #379BFD;
		margin: 0 auto;
		margin-top: 50%;
		position: relative;
		overflow: hidden;
	}

	.main-top-state-fangwei {
		height: 16px;
		position: relative;
		top: 0px;
		width: 1000px;
		border-radius: 10px;
		overflow: hidden;
		margin-left: -280px;
	}

	.main-top-state-fangwei-animation {
		display: inline-table;
		margin-right: 30px;
		width: 30px;
		height: 18px;
		transform: skew(-40deg);
		background-color: #FFE900;
		animation: fangweiAnimation 5s infinite linear;
	}

	@keyframes fangweiAnimation {
		from {
			transform: translateX(0px) skew(-40deg);
		}

		to {
			transform: translateX(299px) skew(-40deg);
		}
	}

	.main-top-state-fangwei-text {
		text-align: center;
		position: absolute;
		top: 0.5px;
		width: 100%;
	}

	.main-top-state-fangwei-text view {
		position: relative;
		background-color: rgba(0, 0, 0, 0.2);
		color: white;
		border-radius: 5px;
		width: 140rpx;
		height: 15px;
		line-height: 15px;
		margin: 0 auto;
		font-size: 20rpx;
		font-weight: bold;
	}

	.main-top-state-date {
		text-align: center;
		position: relative;
		top: 0.8px;
		width: 100%;
	}

	.main-top-state-date view {
		font-size: 20rpx;
		font-weight: bold;
		color: white;
		line-height: 18px;
	}
	.checkSuccessView-success{
		text-align: center;
	}
	.checkSuccessView-success image{
		width: 100rpx;
		height: 100rpx;
		display: block;
		margin: 0 auto;
	}
	.checkSuccessView-success text{
		margin-top: 2px;
		display: block;
		color: white;
		font-weight: bold;
		font-size: 26rpx;
	}
	.checkSuccessView-ul{
		margin-top: 2px;
		color: white;
		font-weight: bold;
		/* background-color: red; */
		padding-left: 32px;
		font-size: 24rpx;
	}
	.exit{
		width: 75rpx;
		height: 75rpx;
/* 		background-color: red; */
		position: absolute;
		top: 8px;
		right: -4px;
	}
</style>
