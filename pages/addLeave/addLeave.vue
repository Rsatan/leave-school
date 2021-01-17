<template>
	<view>
		<label>附件<text class="subGray">(可以选择一张图片作为附件)</text></label>
		<input placeholder="点击设置附件图片" :value="imgFile" @click="clickFile"/>
		<label>你的名字<text class="subRed">*必填</text></label>
		<input placeholder="" v-model="name" />
		<label>请假类型<text class="subRed">*必填</text></label>
		<input placeholder="事假/病假/其他/疫情期间离校请假/外出申请" v-model="type" />
		<label>开始日期/时间<text class="subRed">*必填</text></label>
		<view class="pickerView">
			<picker mode="date" @change="bindStartDate" style="width: 42%;position: absolute;left: 0px;" :value="'2020-' + startDate">
				<view style="line-height: 45px;color: gray;" v-if="startDate == ''">开始日期（年无效）</view>
				<view style="line-height: 45px;" v-if="startDate != ''">{{startDate}}</view>
			</picker>
			<picker mode="time" @change="bindStartTime" style="width: 42%;position: absolute;right: 0px;" :value="startTime">
				<view style="line-height: 45px;color: gray;" v-if="startTime == ''">开始时间</view>
				<view style="line-height: 45px;" v-if="startTime != ''">{{startTime}}</view>
			</picker>
		</view>
		<label>结束日期/时间<text class="subRed">*必填</text></label>
		<view class="pickerView">
			<picker mode="date" @change="bindEndDate" style="width: 42%;position: absolute;left: 0px;" :value="'2020-' + endDate">
				<view style="line-height: 45px;color: gray;" v-if="endDate == ''">结束日期（年无效）</view>
				<view style="line-height: 45px;" v-if="endDate != ''">{{endDate}}</view>
			</picker>
			<picker mode="time" @change="bindEndTime" style="width: 42%;position: absolute;right: 0px;" :value="endTime">
				<view style="line-height: 45px;color: gray;" v-if="endTime == ''">结束时间</view>
				<view style="line-height: 45px;" v-if="endTime != ''">{{endTime}}</view>
			</picker>
		</view>
		<!-- <input placeholder="结束时间:10-11 15:00" v-model="endDate" /> -->
		<label>总时间<text class="subRed">*必填</text></label>
		<input placeholder="共1天3小时" v-model="countDate" />
		<label>紧急联系人<text class="subRed">*必填</text></label>
		<input placeholder="13555555555" v-model="phone" />
		<label>请假原因<text class="subRed">*必填</text></label>
		<input placeholder="(不少于10字)" v-model="reason" />
		<label>抄送人<text class="subRed">*必填</text></label>
		<input placeholder="无" v-model="cc" />
		
		<label>一级审批人<text class="subRed">*必填</text></label>
		<input placeholder="一级审批人姓名" v-model="oneCheck" />
		<label>一级审批人身份<text class="subRed">*必填</text></label>
		<input placeholder="辅导员" v-model="oneCheckStatus" />
		<label>一级审批意见<text class="subRed">*必填</text></label>
		<input placeholder="无" v-model="oneCheckIdea" />
		<label>一级审批通过日期/时间<text class="subRed">*必填</text></label>
		<view class="pickerView">
			<picker mode="date" @change="bindOneCheckDate" style="width: 42%;position: absolute;left: 0px;" :value="'2020-' + oneCheckDate">
				<view style="line-height: 45px;color: gray;" v-if="oneCheckDate == ''">审批日期（年无效）</view>
				<view style="line-height: 45px;" v-if="oneCheckDate != ''">{{oneCheckDate}}</view>
			</picker>
			<picker mode="time" @change="bindOneCheckTime" style="width: 42%;position: absolute;right: 0px;" :value="oneCheckTime">
				<view style="line-height: 45px;color: gray;" v-if="oneCheckTime == ''">审批时间</view>
				<view style="line-height: 45px;" v-if="oneCheckTime != ''">{{oneCheckTime}}</view>
			</picker>
		</view>

		<label>二级审批人<text class="subGray">(如填此项，二级所有信息必填)</text></label>
		<input placeholder="二级审批人姓名" v-model="twoCheck" />
		<label>二级审批人身份</label>
		<input placeholder="院级管理员" v-model="twoCheckStatus" />
		<label>二级审批意见</label>
		<input placeholder="无" v-model="twoCheckIdea" />
		<label>二级审批通过日期/时间</label>
		<view class="pickerView">
			<picker mode="date" @change="bindTwoCheckDate" style="width: 42%;position: absolute;left: 0px;" :value="'2020-' + twoCheckDate">
				<view style="line-height: 45px;color: gray;" v-if="twoCheckDate == ''">审批日期（年无效）</view>
				<view style="line-height: 45px;" v-if="twoCheckDate != ''">{{twoCheckDate}}</view>
			</picker>
			<picker mode="time" @change="bindTwoCheckTime" style="width: 42%;position: absolute;right: 0px;" :value="twoCheckTime">
				<view style="line-height: 45px;color: gray;" v-if="twoCheckTime == ''">审批时间</view>
				<view style="line-height: 45px;" v-if="twoCheckTime != ''">{{twoCheckTime}}</view>
			</picker>
		</view>

		<label>三级审批人<text class="subGray">(如填此项，三级所有信息必填)</text></label>
		<input placeholder="三级审批人姓名" v-model="threeCheck" />
		<label>三级审批人身份</label>
		<input placeholder="校级管理员" v-model="threeCheckStatus" />
		<label>三级审批意见</label>
		<input placeholder="无" v-model="threeCheckIdea" />
		<label>三级审批通过日期/时间</label>
		<view class="pickerView">
			<picker mode="date" @change="bindThreeCheckDate" style="width: 42%;position: absolute;left: 0px;" :value="'2020-' + threeCheckDate">
				<view style="line-height: 45px;color: gray;" v-if="threeCheckDate == ''">审批日期（年无效）</view>
				<view style="line-height: 45px;" v-if="threeCheckDate != ''">{{threeCheckDate}}</view>
			</picker>
			<picker mode="time" @change="bindThreeCheckTime" style="width: 42%;position: absolute;right: 0px;" :value="threeCheckTime">
				<view style="line-height: 45px;color: gray;" v-if="threeCheckTime == ''">审批时间</view>
				<view style="line-height: 45px;" v-if="threeCheckTime != ''">{{threeCheckTime}}</view>
			</picker>
		</view>


		<label>定位位置<text class="subRed">*必填</text></label>
		<input placeholder="中国北京朝阳区/查看位置" v-model="address" />
		
		<label style="display: block;font-weight: bold;">其他设置</label>
		<image src="../../static/img/other_tips.png" style="height:81px;width: 257px;margin: 10px;border: 1px solid #ccc;"></image>
		<checkbox-group @change="changChakan" style="margin-bottom: 10px;">
			<label>
				<checkbox checked="true" value="viewChakan"/>显示"查看>"
			</label>
		</checkbox-group>


		<view class="sub" @click="sub">提交</view>
		<view class="setStuInfo" @click="setStuInfo">设置学生信息（用于扫码签到）</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				leaves: null,
				name: '',
				startDate: '',
				startTime: '',
				endDate: '',
				endTime: '',
				countDate: '',
				reason: '',
				phone: '',
				address: '',
				cc: '',
				oneCheck: '',
				oneCheckDate: '',
				oneCheckTime: '',
				oneCheckStatus:'',
				oneCheckIdea: '无',
				twoCheck: '',
				twoCheckDate: '',
				twoCheckTime: '',
				twoCheckStatus:'',
				twoCheckIdea: '',
				threeCheck: '',
				threeCheckDate: '',
				threeCheckTime: '',
				threeCheckStatus:'',
				threeCheckIdea: '',
				state: '正在休假中',
				type:'',
				colorIndex: 0,
				viewChakan:true,
				imgFile:'',
			}
		},
		onShow() {
			this.leaves = uni.getStorageSync('todaySchoolLeaves');
			if (this.leaves == '' || this.leaves == null) {
				this.leaves = [];
			}
			if(this.leaves.length > 0){
				var leave = this.leaves[this.leaves.length - 1];
				this.name = leave.name;
				this.type = leave.type;
				this.reason = leave.reason;
				this.phone = leave.phone;
				this.address = leave.address;
				this.cc = leave.cc;
				this.oneCheck = leave.oneCheck;
				this.twoCheck = leave.twoCheck;
				this.threeCheck = leave.threeCheck;
				this.oneCheckStatus = leave.oneCheckStatus;
				this.twoCheckStatus = leave.twoCheckStatus;
				this.threeCheckStatus = leave.threeCheckStatus;
				this.oneCheckIdea = leave.oneCheckIdea;
				this.twoCheckIdea = leave.twoCheckIdea;
				this.threeCheckIdea = leave.threeCheckIdea;
			}
		},
		methods: {
			sub: function() {
				var that = this;
				if (this.name == '' || this.type == '' || this.startDate == '' || this.startTime == '' ||
					this.endDate == '' || this.endTime == '' || this.reason == '' || this.address == '' ||
					this.cc == '' || this.phone == '' || this.oneCheckTime == '' || this.oneCheckStatus == '' ||
					this.oneCheck == '' || this.countDate == '' || this.oneCheckDate == '' || this.oneCheckIdea == '') {
					this.toast("请将必填信息填写完整", "none");
					return;
				}
				if (this.startDate == this.endDate && this.startTime == this.endTime) {
					this.toast("开始日期不能等于结束日期", "none");
					return;
				}
				if (this.reason.length < 10) {
					this.toast("请假原因不能少于10字", "none");
					return;
				}
				//二级审批人不为空，但是审批日期或时间、身份、意见为空
				if (this.twoCheck != '' && (this.twoCheckDate == '' || this.twoCheckTime == '' || this.twoCheckStatus == '' || this.twoCheckIdea == '')) {
					this.toast("请将二级审批人审批信息填写完整", "none");
					return;
				}
				//三级审批人不为空，但是审批日期或时间、身份、意见为空
				if (this.threeCheck != '' && (this.threeCheckDate == '' || this.threeCheckTime == '' || this.threeCheckStatus == '' || this.threeCheckIdea == '')) {
					this.toast("请将三级审批人审批信息填写完整", "none");
					return;
				}
				//写三级审批，但是没写二级审批
				if(this.threeCheck != '' && this.twoCheck == ''){
					this.toast("如有三级审批，二级审批必填", "none");
					return;
				}
				var obj = {};
				var date = new Date();
				obj['name'] = this.name;
				obj['type'] = this.type;
				obj['startDate'] = this.startDate + " " + this.startTime;
				obj['endDate'] = this.endDate + " " + this.endTime;
				obj['countDate'] = this.countDate;
				obj['reason'] = this.reason;
				obj['phone'] = this.phone;
				obj['address'] = this.address;
				obj['cc'] = this.cc;
				obj['oneCheck'] = this.oneCheck;
				obj['oneCheckDate'] = this.oneCheckDate + " " + this.oneCheckTime;
				obj['oneCheckStatus'] = this.oneCheckStatus;
				obj['oneCheckIdea'] = this.oneCheckIdea;
				obj['twoCheck'] = this.twoCheck;
				obj['twoCheckDate'] = this.twoCheckDate + " " + this.twoCheckTime;
				obj['twoCheckStatus'] = this.twoCheckStatus;
				obj['twoCheckIdea'] = this.twoCheckIdea;
				obj['threeCheck'] = this.threeCheck;
				obj['threeCheckDate'] = this.threeCheckDate + " " + this.threeCheckTime;
				obj['threeCheckStatus'] = this.threeCheckStatus;
				obj['threeCheckIdea'] = this.threeCheckIdea;
				obj['viewChakan'] = this.viewChakan;
				obj['state'] = this.state;
				obj['applyDate'] = (Number(date.getMonth() + 1).toString() <= 1 ? "0" + Number(date.getMonth() + 1) : Number(date.getMonth() +
						1)) + "-" +
					(date.getDate().toString().length <= 1 ? "0" + date.getDate() : date.getDate()) + " " +
					(date.getHours().toString().length <= 1 ? "0" + date.getHours() : date.getHours()) + ":" +
					(date.getMinutes().toString().length <= 1 ? "0" + date.getMinutes() : date.getMinutes());
			
				obj['imgFile'] = this.imgFile;
				this.leaves.push(obj);
				console.log(this.leaves)
				uni.setStorage({
					key: 'todaySchoolLeaves',
					data: that.leaves,
					success: function() {
						that.toast("提交成功", "success");
						setTimeout(function() {
							uni.navigateBack({
								delta: 1
							})
						}, 1600)
					},
					fail: function() {
						that.toast("提交失败，请重试", "none");
					}
				})
			},

			//跳转到学生信息编辑页面
			setStuInfo: function() {
				uni.navigateTo({
					url: '../addStudentInfo/addStudentInfo'
				})
			},

			//开始日期选择
			bindStartDate: function(e) {
				console.log(e.detail.value.substring(5, e.detail.value.length))
				this.startDate = e.detail.value.substring(5, e.detail.value.length)
			},
			//开始时间选择
			bindStartTime: function(e) {
				console.log(e.detail.value)
				this.startTime = e.detail.value;
			},
			//结束日期选择
			bindEndDate: function(e) {
				console.log(e.detail.value.substring(5, e.detail.value.length))
				this.endDate = e.detail.value.substring(5, e.detail.value.length)
			},
			//结束时间选择
			bindEndTime: function(e) {
				console.log(e.detail)
				this.endTime = e.detail.value;
			},
			//一级审批日期选择
			bindOneCheckDate: function(e) {
				console.log(e.detail.value.substring(5, e.detail.value.length))
				this.oneCheckDate = e.detail.value.substring(5, e.detail.value.length)
			},
			//一级审批时间选择
			bindOneCheckTime: function(e) {
				console.log(e.detail)
				this.oneCheckTime = e.detail.value;
			},
			//二级审批日期选择
			bindTwoCheckDate: function(e) {
				console.log(e.detail.value.substring(5, e.detail.value.length))
				this.twoCheckDate = e.detail.value.substring(5, e.detail.value.length)
			},
			//二级审批时间选择
			bindTwoCheckTime: function(e) {
				console.log(e.detail)
				this.twoCheckTime = e.detail.value;
			},
			//三级审批日期选择
			bindThreeCheckDate: function(e) {
				console.log(e.detail.value.substring(5, e.detail.value.length))
				this.threeCheckDate = e.detail.value.substring(5, e.detail.value.length)
			},
			//三级审批时间选择
			bindThreeCheckTime: function(e) {
				console.log(e.detail)
				this.threeCheckTime = e.detail.value;
			},
			//显示查看多选框被单击
			changChakan:function(e){
				if(e.detail.value[0] == undefined){
					this.viewChakan = false;
				} else {
					this.viewChakan = true;
				}
				console.log(this.viewChakan)
			},
			//实际休假时间颜色选择被单击
			bindColor:function(e){
				console.log('picker发送选择改变，携带值为', e.target.value)
				this.colorIndex = e.target.value;
				this.changColorTitleStyle()
			},
			//实际休假时间颜色标题颜色改变
			changColorTitleStyle:function(){
				if(this.colorIndex == 0){
					return 'line-height: 45px;color:#F79A0D;';
				} else if(this.colorIndex == 1){
					return 'line-height: 45px;color:black;';
				} else if(this.colorIndex == 2){
					return 'line-height: 45px;color:red;';
				}
			},
			//选择附件单击
			clickFile:function(){
				var that = this;
				uni.chooseImage({
					count:1,
					sourceType:['album'],
					success(e) {
						var ff = e.tempFilePaths[0];
						console.log(e.tempFilePaths[0])
						that.toast('附件设置成功','sucess')
						
						uni.saveFile({
							tempFilePath:ff,
							success(res) {
								console.log(res.savedFilePath);
								that.imgFile = res.savedFilePath;
							}
						})
					},
				})
				
			},
			
			toast: function(str, icon) {
				uni.showToast({
					title: str,
					icon: icon,
					duration: 2000,
					mask: true
				})
			}
		}
	}
</script>

<style>
	input,
	picker {
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

	picker:hover {
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
	}

	.setStuInfo {
		background-color: #FFFFFF;
		border: 1px solid #DDDEE2;
		margin: 10px;
		text-align: center;
		font-size: 18px;
		height: 45px;
		line-height: 45px;
		border-radius: 5px;
		margin-bottom: 350px;
	}

	.pickerView {
		width: 100%;
		position: relative;
		height: 45px;
		margin-bottom: 20px;
		text-align: center;
	}

	.pickerView picker {
		z-index: 9999;
		background-color: white;
	}

	.border-center {
		border-top: 1px solid gray;
		width: 50%;
		margin: 0 auto;
		position: relative;
		top: 25px;
	}

	.subRed {
		font-size: 20rpx;
		color: red;
	}
	.subGray{
		font-size: 20rpx;
		color: #ffa400;
	}
</style>
