<template>
	<div class="main">
		<div>
			<uni-icons @click="nextM(-1)" type="left"></uni-icons>
			<span>{{ currentM.format('YYYY-MM') }}</span>
			<uni-icons @click="nextM(1)" type="right"></uni-icons>
		</div>
		<view class="a">
			<div class="b t" v-for="(w, wi) in 7" :key="wi">周{{ week[w] }}</div>
			<view v-if="currentM.date(0).day() + 1 < 7" class="b" v-for="d in currentM.date(0).day() + 1" :key="d"></view>
			<view :class="['b', events.includes(d + 1) ? 'c' : '']" v-for="d in currentM.daysInMonth()" :key="d" @click="navTo(d + 1)">
				<view>{{ d + 1 }}</view>
				<div class="point" v-if="events.includes(d + 1)"></div>
			</view>
		</view>
	</div>
</template>

<script>
import dayjs from 'dayjs';
import { week } from '@/constant/index.js';
export default {
	data() {
		return {
			dayjs: dayjs,
			week: week,
			events: [],
			currentM: dayjs()
		};
	},
	onLoad() {
		// uni.request({
		// 	url: 'https://ckcalendar.952698119.workers.dev/',
		// 	success: res => {
		// 		this.events = res.data.list;
		// 	}
		// });
		this.events = [1, 23, 6, 27];
	},
	methods: {
		navTo(item) {
			uni.navigateTo({
				url: `/pages/list/list?date=${dayjs(this.currentM).format('YYYY-MM-' + item)}`
			});
		},
		nextM(i) {
			this.currentM = this.currentM.add(i, 'month');
		}
	}
};
</script>

<style lang="scss" scoped>
.main {
	display: flex;
	flex-direction: column;
	//align-items: center;
	width: 750rpx;
}
.a {
	display: grid;
	grid-template-columns: 1fr 1fr 1fr 1fr 1fr 1fr 1fr;
	.b {
		display: inline-block;
		background-color: #dee0e6;
		box-shadow: 0 2px 6px 0;
		border-radius: 14rpx;
		height: 100rpx;
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: space-around;
		.point {
			width: 12rpx;
			height: 12rpx;
			border-radius: 50%;
			background-color: #36cfc9;
		}
	}
	.c {
		background-color: rgba(90, 160, 245, 0.85);
	}
	.t {
		justify-content: center;
		height: 50rpx;
	}
}
</style>
