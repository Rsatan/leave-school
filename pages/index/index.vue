<template>
	<view class="container">
		<!-- 上方显示时间 显示wifi的通栏 -->
		<view class="status_bar">
			<view class="top_view"></view>
		</view>
		<view class="nav">
			<view class="nav-left">
				<image src="../../static/img/dt_store_back.png"></image>
			</view>
			<view class="nav-title">
				<image src="../../static/img/dun.png"></image>辅导猫
			</view>
			<view class="nav-right" @click="clearAll" @touchstart="touchStart" @touchend="touchEnd">反馈</view>
		</view>
		<view class='tab-box'>
			<view class="check-left"><view class="check-on">请假记录</view></view>
			<view class="check-right">无需审批记录</view>
		</view>

		<view class="main">
			<view class="main-top-tips">
				<image src="../../static/img/wenhao.png" class="main-top-tips-image1"></image>
				为何不能请假？
				<!-- <image src="../../static/img/right.png" class="main-top-tips-image2"></image> -->
			</view>
			<view class="main-content" v-for="(item,i) in leaves" @click="clickContent(i)">
				<view class="main-content-title">我的 {{item.type}}申请</view>
				<view class="main-content-date">{{item.startDate}}</view>
				<view class="main-content-startDate">请假时间：{{item.startDate}} 至 {{item.endDate}} ({{item.countDate}})</view>
				<view class="main-content-startDate" v-if="item.state == '已完成'">实际休假时间：<text :style="getDateStyle(i)">{{item.startDate}} ~ {{item.endDate}} ({{item.countDate}})</text></view>
				<view class="main-content-state" :style="getStateStyle(item.state)">{{item.state}}</view>
			</view>
			<view class="main-bottom-tips">没有更多了</view>
		</view>
		<view class="bottom-btn" @click="addLeave">我要请假</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				title: 'Hello',
				leaves: null,
				loop:null,
				studentInfo:null,
			}
		},
		onShow() {
			this.leaves = uni.getStorageSync('todaySchoolLeaves');
			this.leaves.reverse();
			console.log(this.leaves);
		},
		
		onBackPress(options) {
				if (options.from === 'navigateBack') {
					return false;
				}
				this.onreturn();
				return true;
			},
		
		methods: {
			onreturn(){
				uni.redirectTo({
					url:'./index'
				});
			},
			
			//添加请假记录
			addLeave: function() {
				uni.navigateTo({
					url: '../addLeave/addLeave'
				});
			},
			//删除所有请假记录
			clearAll: function() {
				uni.showModal({
					title: '提示',
					content: '确定要删除所有记录吗？',
					success: function(e) {
						if (e.confirm) {
							uni.setStorageSync("todaySchoolLeaves", null);
							this.leaves = null;
							uni.reLaunch({
								url: '../index/index'
							})
						}
					}
				})
			},
			//根据请假状态，返回状态颜色
			getStateStyle: function(str) {
				// 	if(str == '正在休假中'){
				// 		return 'color:#979AA1;';
				// 	}
				return 'color:#A0A9B6;';
			},
			//点击查看请假记录
			clickContent: function(i) {
				console.log(this.leaves[i]);
				uni.setStorageSync("currLeaves", this.leaves[i]);
				uni.setStorageSync("currIndex", i);
				uni.navigateTo({
					url: '../viewLeaves/viewLeaves',
					animationType:'none'
				})
			},

			//长按开始
			touchStart: function() {
				var that = this;
				//再次清空定时器，防止重复注册定时器
				clearTimeout(this.loop); 
				this.loop = setTimeout(() => {
					//扫码前先判断是否已经设置学生信息
					that.studentInfo = uni.getStorageSync("studentInfo");
					if(that.studentInfo == '' || that.studentInfo == null || that.studentInfo == undefined){
						uni.showToast({
							title:'扫码前需先设置个人信息',
							duration:2000,
							icon:'none'
						})
						uni.navigateTo({
							url:'../addStudentInfo/addStudentInfo'
						})
					} else {
						uni.scanCode({
							success:function(e){
								console.log(e.result)
								uni.navigateTo({
									url:'../checkSuccess/checkSuccess'
								})
							}
						})
					}
				}, 700);
			},
			//跳转今日校园APP
			// openApp:function(){
			// 	plus.runtime.launchApplication({
			// 		pname:'com.wisedu.cpdaily'
			// 	})
			// },
			//长按结束
			touchEnd: function() {
				//清空定时器，防止重复注册定时器
				clearTimeout(this.loop); 
			},
			
			//设置实际休假时间颜色，双数为灰色，单数为黄
			getDateStyle:function(i){
				if(i % 2 != 0){
					return "color:#F59A12;"
				}
			}
		}
	}
</script>

<style>
	page {
		background-color: #FFFFFF;
	}

	.container {
		padding: 20px;
		padding-left: 0px;
		padding-right: 0px;
		font-size: 22rpx;
		line-height: 24px;
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
		height: 70px;
		background: white;
		border-bottom: 1px solid #efefef;
		z-index: 9999;
	}

	.nav-left {
		padding-top: 34px;
		margin-left: 28rpx;
	}

	.nav-left image {
		width: 20px;
		height: 20px;
	}

	.nav-title {
		font-weight: 450;
		font-size: 33rpx;
		letter-spacing: 1.5px;
		position: absolute;
		left: calc(50% - 78rpx);
		top: 52rpx;
	}

	.nav-title image {
		width: 48rpx;
		height: 50rpx;
		position: relative;
		top: 14rpx;
		left: -4rpx;
	}

	.nav-right {
		position: absolute;
		right: 36rpx;
		top: 35px;
		font-size: 33rpx;
		color: #41E0EB;
	}
	
	.tab-box {
		position: relative;
		width: 100%;
		height: 20px;
		top: 90px;
		background: white;
		border-bottom: 1px solid #efefef;
		z-index: 999;
	}
	
	.check-left {
		position: absolute;
		width: 50%;
		height: 20px;
		text-align: center;
		font-size: 30rpx;
		color:#3399FE;
		top: -10px;
		left: 60px;
	}
	
	.check-right {
		position: absolute;
		width: 50%;
		height: 20px;
		text-align: center;
		font-size: 30rpx;
		color:#b0b1b5;
		right: 10px;
		top: -10px;
	}

	.check-on {
		position: absolute;
		height: 28px;
		width: 80px;
		font-weight: 500;
		color:#3399FE;
		border-bottom: 3px solid #3399FE;
	}
	
	.main {
		position: relative;
		top: 90px;
	}

	.main-bottom-tips {
		text-align: center;
		margin-top: 30rpx;
		font-size: 24rpx;
		padding-bottom: 115rpx;
		color: #A7AFBA;
	}

	.main-top-tips {
		width: 100%;
		height: 45rpx;
		background-color: #FE9900;
		text-align: center;
		color: white;
		font-size: 24rpx;
		line-height: 45rpx;
		position: relative;
	}

	.main-top-tips-image1 {
		width: 15px;
		height: 15px;
		position: absolute;
		top: 9rpx;
		left: calc(50% - 120rpx);
	}

	.main-top-tips-image2 {
		width: 22px;
		height: 20px;
		position: absolute;
		top: 6px;
	}

	.main-content {
		padding-top: 24px;
		padding-bottom: 22px;
		padding-left: 22px;
		padding-right: 22px;
		background-color: white;
		border-bottom: 1px solid #F1F2F4;
	}

	.main-content-title {
		font-size: 17px;
		display: inline-block;
		color: #464C5C;
	}

	.main-content-date {
		font-size: 27rpx;
		float: right;
		color: #9EA8B4;
	}

	.main-content-startDate {
		font-size: 26rpx;
		color: #A1A9B4;
		margin-top: 10px;
	}

	.main-content-state {
		margin-top: 8px;
		font-size: 26rpx;
	}

	.bottom-btn {
		position: fixed;
		bottom: 0px;
		background-color: #3399FE;
		width: 100%;
		height: 85rpx;
		color: white;
		text-align: center;
		line-height: 85rpx;
		font-size: 32rpx;
	}
</style>
