<template>
	<view class="user">
		<view class="top">
			<image src="../../static/images/history.png" mode=""></image>
			<view class="text">浏览历史</view>
			<view class="clean">
				<text @click="cleanHistory">清除浏览记录</text>
			</view>
		</view>
		<view class="content">
			<view class="row" v-for="item in listArr">
				<newsbox :item="item" @click.native="goDetail(item)"></newsbox>
			</view>
		</view>
		<view class="nohistory" v-if="!listArr.length">
			<image src="../../static/images/nohis.png" mode="widthFix"></image>
			<view class="text">暂无浏览记录</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				listArr: []
			};
		},
		onShow() {
			this.getData()
		},
		methods: {
			// 清除浏览记录
			cleanHistory() {
				if(this.listArr.length === 0) return
				uni.clearStorageSync()
				uni.showLoading()
				setTimeout(() => {
					this.getData()
					// 关闭loding
					uni.hideLoading()
				}, 1500)
			},
			//跳转到详情页
			goDetail(item) {
				uni.navigateTo({
					url: `/pages/detail/detail?cid=${item.classid}&id=${item.id}`
				})
			},
			//获取缓存浏览记录
			getData() {
				let hisArr = uni.getStorageSync("historyArr") || []
				this.listArr = hisArr
			}
		}
	}
</script>

<style lang="scss">
	.user {
		.top {
			padding: 50rpx 0;
			background: #F8F8F8;
			color: #666;
			display: flex;
			flex-direction: column;
			justify-content: center;
			align-items: center;

			image {
				width: 150rpx;
				height: 150rpx;
			}

			.text {
				font-size: 38rpx;
				padding-top: 20rpx;
			}

			.clean {
				height: 40rpx;
				width: 700rpx;
				text-align: right;

				text {
					width: 230rpx;
					height: 50rpx;
					line-height: 50rpx;
					margin-top: 20rpx;
					display: inline-block;
					text-align: center;
					background-color: #ddd;
					color: #666;
					border-radius: 20rpx;
					font-size: 30rpx;
				}
			}
		}

		.content {
			padding: 30rpx;

			.row {
				border-bottom: 1px dotted #efefef;
				padding: 20rpx 0;
			}
		}

		.nohistory {
			display: flex;
			flex-direction: column;
			justify-content: center;
			align-items: center;

			image {
				width: 450rpx;
			}

			.text {
				font-size: 26rpx;
				color: #888;
			}
		}
	}
</style>
