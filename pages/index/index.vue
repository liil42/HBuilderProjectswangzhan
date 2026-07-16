<template>
	<view class="content">
		<view class="head">班级点名系统</view>
		<view class="num-text" v-if="currentId">当前抽到学号：{{currentId}}</view>
		<view class="empty-tip" v-else>点击开始点名</view>

		<view class="btn-box">
			<button class="start-btn" @click="startRoll" :disabled="isRolling">
				{{ isRolling ? "正在抽取" : "开始点名" }}
			</button>
			<button class="reset-btn" @click="resetAll">重置全部</button>
		</view>

		<view class="list-title">已经点到的学号</view>
		<view class="id-list">
			<view class="item" v-for="item in alreadyList" :key="item">{{item}}</view>
		</view>
	</view>
</template>

<script setup>
import { ref } from 'vue'
const currentId = ref('')
const isRolling = ref(false)
const alreadyList = ref([])
let timer = null

const startRoll = () => {
	if (alreadyList.value.length >= 50) {
		alert('全部学号抽取完毕，请重置！')
		return
	}
	isRolling.value = true
	timer = setInterval(() => {
		let randomNum = Math.floor(Math.random() * 50) + 1
		currentId.value = randomNum
	}, 80)

	setTimeout(() => {
		clearInterval(timer)
		let finalNum = Math.floor(Math.random() * 50) + 1
		while (alreadyList.value.includes(finalNum)) {
			finalNum = Math.floor(Math.random() * 50) + 1
		}
		currentId.value = finalNum
		alreadyList.value.push(finalNum)
		isRolling.value = false
	}, 2000)
}

const resetAll = () => {
	clearInterval(timer)
	currentId.value = ''
	alreadyList.value = []
	isRolling.value = false
}
</script>

<style scoped>
.content {
	padding: 40rpx;
}
.head {
	font-size: 44rpx;
	font-weight: bold;
	text-align: center;
	margin: 40rpx 0;
	color: #333;
}
.num-text {
	font-size: 60rpx;
	text-align: center;
	color: #e53e3e;
	font-weight: bold;
	margin: 80rpx 0;
}
.empty-tip{
	font-size: 30rpx;
	text-align: center;
	color: #999;
	margin:80rpx 0;
}
.btn-box {
	display: flex;
	gap: 20rpx;
}
.start-btn {
	flex: 1;
	background-color: #007aff;
	color: #fff;
}
.reset-btn {
	flex:1;
	background-color: #666666;
	color: #ffffff;
}
.list-title {
	font-size: 32rpx;
	margin:60rpx 0 20rpx;
}
.id-list {
	display: flex;
	flex-wrap: wrap;
	gap: 15rpx;
}
.item {
	width: 70rpx;
	height:70rpx;
	line-height:70rpx;
	text-align: center;
	background-color: #f0f4ff;
	border-radius:10rpx;
	font-size:26rpx;
}
</style>
