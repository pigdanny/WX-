<template>
	<view>
		<!-- top -->
		<view class="img">
			<text>{{namearr[index]}}</text>
		</view>
		<!-- 榜单 -->
		<view class="list" v-for="(item,index) in datalist" :key="index" @tap="click(item.id)">
			<view class="top">No.{{index+1}}</view>
			<view class="mid">
				<view class="mid_img">
					<image :src="item.images.large"></image>
				</view>
				<view class="mid_msg">
					<text>{{item.title}}</text>
					<view class="star">
						<uni-rate :value="item.rating.average/2" size="16" allowHalf readonly />
						<text>{{item.rating.average.toFixed(1)}}</text>
					</view>
					<view class="mid_msgs">
						<text>{{item.countries[0]}}</text>/
						<text v-for="(item1,index) in item.genres" :key="index">{{item1}}</text>/
						<text>{{item.directors[0].name}}</text>/
						<text v-for="(item2,index) in item.casts" :key="index">{{item2.name}}、</text>
					</view>
				</view>
			</view>
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
				num: 1,
				index: 0,
				namearr: [
					"最近热映", "即将上映", "TOP250"
				],
				datalist: [],
				apiarr: [{
					url: "http://t.yushu.im/v2/movie/in_theaters?apikey=0df993c66c0c636e29ecbb5344252a4a"
				}, {
					url: "http://t.yushu.im/v2/movie/coming_soon?apikey=0df993c66c0c636e29ecbb5344252a4a"
				}, {
					url: "http://t.yushu.im/v2/movie/top250?apikey=0df993c66c0c636e29ecbb5344252a4a"
				}]
			}
		},
		onLoad(obj) {
			this.index = obj.index
			this.req(obj.index)
		},
		onPullDownRefresh() {
			this.req(this.index)
			uni.stopPullDownRefresh()
		},
		onReachBottom() {
			this.num += 10
			this.req(this.index)
		},
		methods: {
			/* 发送请求 */
			req(index) {
				uni.request({
					url: this.apiarr[index].url,
					data: {
						start: this.num,
						count: 10
					},
					success: (res) => {
						this.datalist = this.datalist.concat(res.data.subjects)
					},
				});
			},
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
	.img {
		text-align: center;
		line-height: 200px;
		background-color: #333340;
		color: #DFA14F;
		font-size: 40px;
	}

	.list {
		flex: 1;
		display: flex;
		flex-direction: column;
		padding: 10px;

		.top {
			width: 50px;
			line-height: 25px;
			border-radius: 5px;
			font-size: 16px;
			text-align: center;
			background-color: #DFA14F;
		}

		.mid {
			display: flex;

			.mid_img {
				width: 120px;
				height: 150px;
				border-radius: 5px;
				padding: 5px 0;

				image {
					width: 120px;
					height: 150px;
				}
			}

			.mid_msg {
				flex: 1;
				font-weight: bold;
				font-size: 16px;
				margin-left: 10px;
			}

			.star {
				display: flex;
				align-items: center;
				margin-top: 10px;

				text {
					font-size: 12px;
					margin-left: 5px;
					color: #888888;
				}
			}

			.mid_msgs {
				color: #888888;
				font-size: 14px;
				margin-top: 10px;

				.accter {
					white-space: pre-wrap;
				}
			}
		}
	}
</style>
