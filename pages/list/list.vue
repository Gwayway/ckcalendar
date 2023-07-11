<template>
	<view>
		<div>{{ date }}</div>
		<uni-icons type="folder-add-filled" size="30" @click="add"></uni-icons>
		<div class="list-main">
			<div class="list-item" v-for="(e, i) in eventList" :key="i">
				<div class="tt-ac">
					<div :class="['tt', e.isdeal == 1 ? 'deal' : '']">{{ e.title }}</div>
					<div class="ac">
						<div v-if="!e.isdeal" class="sc" @click="deal(e)">完成</div>
						<div class="de" @click="delet(e)">删除</div>
					</div>
				</div>
				<uni-icons type="bottom" @click="dshow(e)"></uni-icons>
				<div v-if="e.show">{{ e.message }}</div>
			</div>
		</div>
		<uni-popup ref="popup" type="top">
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
		if (e.has == 'true') {
			this.get();
		}
	},
	methods: {
		dshow(item) {
			if (!item.hasOwnProperty('show')) {
				this.$set(item, 'show', false);
			}
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
					this.get();
					this.$refs.popup.close();
				}
			});
		},
		deal(item) {
			uni.request({
				url: 'https://mycalendarserver.bestwill.workers.dev/deal',
				method: 'POST',
				data: { id: item.id },
				success: res => {
					this.get();
				}
			});
		},
		delet(item) {
			uni.request({
				url: 'https://mycalendarserver.bestwill.workers.dev/delet',
				method: 'POST',
				data: { id: item.id },
				success: res => {
					this.get();
				}
			});
		},
		get() {
			uni.request({
				url: 'https://mycalendarserver.bestwill.workers.dev/get',
				method: 'POST',
				data: { date: this.date },
				success: res => {
					this.eventList = res.data;
				}
			});
		}
	}
};
</script>

<style lang="scss" scoped>
.deal {
	text-decoration: line-through double red;
}
.list-main {
	display: flex;
	flex-direction: column;
	padding: 12rpx;
	.list-item {
		background-color: #eeeeee;
		min-height: 80rpx;
		margin-bottom: 12rpx;
		.tt-ac {
			display: flex;
			justify-content: space-between;
			.tt {
			}

			.ac {
				font-size: 24rpx;
				display: flex;
				> div {
					padding: 10rpx;
					margin: 10rpx;
				}
				.sc {
					background-color: greenyellow;
				}
				.de {
					background-color: rosybrown;
				}
			}
		}
	}
}
</style>
