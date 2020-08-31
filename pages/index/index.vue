<template>
	<view>
		<!-- 搜索 -->
		<view class="search">
			<navigator url="/pages/search/search">
				<view class="search_input">搜索</view>
			</navigator>
		</view>
		<!-- 榜单 -->
		<view class="list" v-for="(data,index) in datalist" :key="index">
			<view class="title">
				<text>{{namearr[index]}}</text>
				<text>查看更多></text>
			</view>
			<scroll-view class="lists" scroll-x>
				<view class="box" v-for="(item,subindex) in data.arr" :key="subindex" @tap="click(item.id)">
					<image :src="item.images.large"></image>
					<view class="name">{{item.title}}</view>
					<view class="star">
						<uni-rate :value="item.rating.average/2" size="16" allowHalf readonly />
						<text>{{item.rating.average.toFixed(1)}}</text>
					</view>
				</view>
			</scroll-view>
		</view>
	</view>
</template>

<script>
	import {
		uniRate
	} from '@dcloudio/uni-ui';

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
					url: "http://t.yushu.im/v2/movie/in_theaters?apikey=0df993c66c0c636e29ecbb5344252a4a&start=1&count=10"
				}, {
					url: "http://t.yushu.im/v2/movie/coming_soon?apikey=0df993c66c0c636e29ecbb5344252a4a&start=1&count=10"
				}, {
					url: "http://t.yushu.im/v2/movie/top250?apikey=0df993c66c0c636e29ecbb5344252a4a&start=0&count=10"
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
			/* 电影详情 */
			click(id) {
				uni.navigateTo({
					url: '/pages/movieinfo/movieinfo?id=' + id
				});
			}
		},
		components: {
			uniRate
		}
	}
</script>

<style lang="less" scoped>
	.search {
		padding: 10px;
		background-color: #43BD56;

		.search_input {
			padding: 5px;
			border-radius: 5px;
			padding-left: 10px;
			font-size: 14px;
			color: #C0C0C0;
			text-align: center;
			background-color: #FFFFFF;
		}

	}

	.list {
		display: flex;
		flex-direction: column;
		padding: 10px;

		.title {
			display: flex;
			justify-content: space-between;
			align-items: center;

			text {
				font-size: 14px;

				&:first-child {
					font-weight: bold;
				}

				&:last-child {
					color: #43BD56;
				}
			}
		}

		.lists {
			width: 100%;
			white-space: nowrap;

			.box {
				display: inline-block;
				margin-right: 10px;
				margin-top: 20px;

				image {
					width: 150px;
					height: 200px;
					border-radius: 5px;
				}

				.name {
					font-weight: bold;
					font-size: 14px;
					width: 120px;
					overflow: hidden;
					text-overflow: ellipsis;
					white-space: nowrap;
				}

				.star {
					display: flex;
					align-items: center;

					text {
						font-size: 12px;
						margin-left: 5px;
						color: #888888;
					}
				}
			}
		}
	}
</style>
