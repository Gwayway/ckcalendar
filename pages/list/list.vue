<template>
	<view>
		<div>{{ date }}</div>
		<uni-icons type="folder-add-filled" size="30" @click="add"></uni-icons>
		<div class="list-main">
			<div class="list-item" v-for="(e, i) in eventList" :key="i">
				<div>{{ e.title }}</div>
				<uni-icons type="bottom" @click="dshow(e)"></uni-icons>
				<div>{{ e.message }}</div>
			</div>
		</div>
		<uni-popup ref="popup" type="center">
			<div>
				<uni-easyinput v-model="addObj.title" />
				<uni-easyinput v-model="addObj.message" type="textarea" />
				<button @click="submit">确认</button>
			</div>
		</uni-popup>
	</view>
</template>

<script>
export default {
	data() {
		return {
			date: '',
			eventList: [],
			addObj: {
				title: '',
				show: '',
				message: ''
			}
		};
	},
	onLoad(e) {
		this.date = e.date;
		uni.request({
			url: 'https://mycalendarserver.bestwill.workers.dev/get',
			method: 'POST',
			data: { date: this.date},
			success: res => {
				this.eventList = res.data;
			}
		});
	},
	methods: {
		dshow(item) {
			item.show = !item.show;
		},
		add() {
			Object.keys(this.addObj).forEach(key => {
				this.addObj[key] = '';
			});
			this.$refs.popup.open();
		},
		submit() {
			uni.request({
				url: 'https://mycalendarserver.bestwill.workers.dev/add',
				method: 'POST',
				data: { date: this.date, ...this.addObj },
				success: res => {
					console.log(res);
				}
			});
			this.eventList.push({ ...this.addObj });
			this.$refs.popup.close();
		}
	}
};
</script>

<style lang="scss" scoped>
.list-main {
	display: flex;
	flex-direction: column;
	padding: 12rpx;
	.list-item {
		background-color: #eeeeee;
		min-height: 80rpx;
		margin-bottom: 12rpx;
	}
}
</style>
