<template>
	<view>
		<div>{{ date }}</div>
		<uni-icons type="folder-add-filled" size="30" @click="add"></uni-icons>
		<div class="list-main">
			<div class="list-item" v-for="(e, i) in eventList" :key="i">
				<div>{{ e.title }}</div>
				<uni-icons type="bottom" @click="dshow(e)"></uni-icons>
				<div v-if="e.show">{{ e.detail }}</div>
			</div>
		</div>
		<uni-popup ref="popup" type="center">
			<div>
				<uni-easyinput v-model="addObj.title" />
				<uni-easyinput v-model="addObj.detail" type="textarea" />
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
			eventList: [
				{
					title: '第一个',
					show: false,
					detail: '爱国覅ask i规划洛克菲勒'
				},
				{
					title: '第二个',
					show: false,
					detail: '韩国发i啊是个体覅'
				},
				{
					title: '第三个',
					show: false,
					detail: '阿红覅哈多方了解'
				}
			],
			addObj: {
				title: '',
				show: '',
				detail: ''
			}
		};
	},
	onLoad(e) {
		this.date = e.date;
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
