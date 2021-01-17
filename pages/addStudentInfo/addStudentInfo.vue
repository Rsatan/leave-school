<template>
	<view>
		<label>姓名<text class="subRed">*必填</text></label>
		<input placeholder="你的姓名" v-model="name" />
		<label>学号<text class="subRed">*必填</text></label>
		<input placeholder="你的学号" v-model="stuId" />
		<label>性别<text class="subRed">*必填</text></label>
		<input placeholder="你的性别" v-model="sex" />
		<label>民族<text class="subRed">*必填</text></label>
		<input placeholder="你的民族" v-model="nation" />
		<label>年级<text class="subRed">*必填</text></label>
		<input placeholder="2020级" v-model="year" />
		<label>学院<text class="subRed">*必填</text></label>
		<input placeholder="计算机科学院" v-model="system" />
		<label>专业<text class="subRed">*必填</text></label>
		<input placeholder="软件工程" v-model="subject" />
		<label>班级<text class="subRed">*必填</text></label>
		<input placeholder="你的班级号" v-model="classNum" />
		<label>宿舍<text class="subRed">*必填</text></label>
		<input placeholder="你的宿舍" v-model="dorm" />
		<view class="sub" @click="sub">提交</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				studentInfo:null,
				name:'',
				stuId:'',
				sex:'',
				nation:'',
				year:'',
				system:'',
				subject:'',
				classNum:'',
				dorm:'',
			}
		},
		onShow() {
			this.studentInfo = uni.getStorageSync("studentInfo");
			if(this.studentInfo != '' && this.studentInfo != null && this.studentInfo != undefined){
				this.name = this.studentInfo.name;
				this.stuId = this.studentInfo.stuId;
				this.sex = this.studentInfo.sex;
				this.nation = this.studentInfo.nation;
				this.year = this.studentInfo.year;
				this.system = this.studentInfo.system;
				this.subject = this.studentInfo.subject;
				this.classNum = this.studentInfo.classNum;
				this.dorm = this.studentInfo.dorm;
			} 
		},
		methods: {
			sub:function(){
				if(this.name == '' || this.stuId == '' || this.sex == '' || this.nation == '' || this.year == ''||
					this.system == '' || this.subject == '' || this.classNum == '' || this.dorm == ''){
						this.toast("请将信息填写完整", "none");
						return ;
				}
				var stuInfo = {};
				stuInfo['name'] = this.name;
				stuInfo['stuId'] = this.stuId;
				stuInfo['sex'] = this.sex;
				stuInfo['nation'] = this.nation;
				stuInfo['year'] = this.year;
				stuInfo['system'] = this.system;
				stuInfo['subject'] = this.subject;
				stuInfo['classNum'] = this.classNum;
				stuInfo['dorm'] = this.dorm;
				uni.setStorageSync("studentInfo",stuInfo);
				this.toast("设置成功","success")
				setTimeout(function(){
					uni.navigateBack({
						delta:1
					})
				},2000)
				
			},
			toast: function(str, icon) {
				uni.showToast({
					title: str,
					icon: icon,
					duration: 1600,
					mask: true
				})
			}
		}
	}
</script>

<style>
	input {
		border: 1px solid #ccc;
		margin: 10px;
		margin-top: 2px;
		margin-bottom: 10px;
		padding-left: 5px;
		padding-right: 5px;
		height: 45px;
		border-radius: 5px;
		transition: all .3s ease;
	}

	input:hover {
		border: 1px solid #007AFF;
		box-shadow: 0px 0px 5px 1px #eeeeee;
	}
	label {
		margin-left: 10px;
		color: #2c906f;
	}
	.sub {
		background-color: #38b48b;
		margin: 10px;
		margin-top: 30px;
		color: white;
		text-align: center;
		font-size: 18px;
		height: 45px;
		line-height: 45px;
		border-radius: 5px;
		margin-bottom: 350px;
	}
	.subRed {
		font-size: 20rpx;
		color: red;
	}
</style>
