<template>
	<view>
		<view class="list" v-for="(itemlist,index) in datalist" :key="index" @tap="click(index)">
			<view class="img">{{namearr[index]}}</view>
			<view class="sublist">
				<view v-for="(item,subindex) in itemlist.arr" :key="subindex">
					<text class="name">{{subindex+1}}.{{item.title}}</text>
					<text class="rate">{{item.rating.average.toFixed(1)}}分</text>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				namearr: [
					"最近热映", "即将上映", "TOP250"
				],
				datalist: [{
						arr: []
					},
					{
						arr: []
					},
					{
						arr: []
					}
				],
				apiarr: [{
					url: "http://t.yushu.im/v2/movie/in_theaters?apikey=0df993c66c0c636e29ecbb5344252a4a&start=1&count=3"
				}, {
					url: "http://t.yushu.im/v2/movie/coming_soon?apikey=0df993c66c0c636e29ecbb5344252a4a&start=1&count=3"
				}, {
					url: "http://t.yushu.im/v2/movie/top250?apikey=0df993c66c0c636e29ecbb5344252a4a&start=1&count=3"
				}]
			}
		},
		onLoad() {
			for (let i = 0; i < this.apiarr.length; i++) {
				uni.request({
					url: this.apiarr[i].url,
					success: (res) => {
						this.datalist[i].arr = res.data.subjects
					}
				});
			}
		},
		onPullDownRefresh() {
			for (let i = 0; i < this.apiarr.length; i++) {
				uni.request({
					url: this.apiarr[i].url,
					success: (res) => {
						this.datalist[i].arr = res.data.subjects
						uni.stopPullDownRefresh();
					}
				});
			}
		},
		methods: {
			click(index) {
				uni.navigateTo({
					url: '/pages/listinfo/listinfo?index=' + index
				});
			}
		}
	}
</script>

<style lang="less" scoped>
	.list {
		display: flex;
		align-items: center;
		padding: 10px;

		.img {
			width: 100px;
			height: 100px;
			line-height: 100px;
			border-radius: 5px;
			font-size: 16px;
			color: #FFFFFF;
			background-color: #38434D;
			text-align: center;
		}

		.sublist {
			flex: 1;
			display: flex;
			flex-direction: column;
			height: 80px;
			font-size: 16px;
			margin: 20px;
			margin-left: 0;
			padding-left: 20px;

			>view {
				display: flex;
				justify-content: space-between;
				align-items: center;
				line-height: 30px;

				.name {
					display: inline-block;
					width: 300rpx;
					overflow: hidden;
					text-overflow: ellipsis;
					white-space: nowrap;
				}

				.rate {
					color: #DE9169;
				}
			}
		}
	}
</style>
