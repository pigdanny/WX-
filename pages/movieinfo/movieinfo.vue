<template>
	<view class="movieinfo">
		<!-- 电影详情 -->
		<view class="movie">
			<image :src="datalist.images.large"></image>
			<view class="movie_msg">
				<view>{{datalist.title}}({{datalist.year}})</view>
				<view>{{datalist.original_title}}({{datalist.year}})</view>
				<view>
					<text v-for="(item,index) in datalist.genres" :key="index">
						{{item}}、
					</text>
					<text>{{datalist.countries[0]}}</text>
				</view>
				<view class="btn">
					<text>想看</text>
					<text>看过</text>
				</view>
			</view>
		</view>
		<!-- 电影评分 -->
		<view class="movie_rate">
			<text>豆豆评分</text>
			<view class="star">
				<text>{{datalist.rating.average.toFixed(1)}}</text>
				<uni-rate :value="datalist.rating.average/2" size="14" allowHalf readonly />
			</view>
		</view>
		<!-- 电影简介 -->
		<view class="movie_msg">
			<text>简介</text>
			<view>{{datalist.summary}}</view>
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
				id:0,
				url: "http://t.yushu.im/v2/movie/subject/",
				datalist: []
			}
		},
		onLoad(obj) {
			uni.showLoading({
				title: '加载中'
			});
			this.id=obj.id
			this.req(obj.id)
		},
		onPullDownRefresh() {
			this.req(this.id)
			uni.stopPullDownRefresh()
		},
		methods: {
			req(id) {
				uni.request({
					url: this.url + id + "?apikey=0df993c66c0c636e29ecbb5344252a4a",
					data: {
						start: 1,
						count: 10
					},
					success: (res) => {
						this.datalist = res.data
						uni.hideLoading()
					},
				});
			},
		},
		components: {
			uniRate
		}
	}
</script>

<style lang="less" scoped>
	.movieinfo {
		display: flex;
		flex-direction: column;
		padding: 10px;
		margin-top: 10px;
		background-color: #F5F6F8;

		.movie {
			display: flex;

			image {
				width: 110px;
				height: 150px;
				border-radius: 5px;
			}

			.movie_msg {
				margin-left: 10px;
				flex: 1;
				background-color: #F5F6F8;

				view {
					&:nth-child(1) {
						font-weight: bold;
						font-size: 16px;
					}

					&:nth-child(2) {
						color: #A0A1A3;
						font-size: 12px;
						margin-top: 5px
					}

					&:nth-child(3) {
						color: #A0A1A3;
						font-size: 10px;
						margin: 10px 0 20px;
					}
				}

				.btn {
					display: flex;
					justify-content: space-around;
					align-items: center;

					text {
						display: inline-block;
						width: 100px;
						height: 30px;
						line-height: 30px;
						font-size: 14px;
						font-weight: bold;
						text-align: center;
						background-color: #ffffff;
						border-radius: 5px;
					}
				}
			}
		}

		.movie_rate {
			margin-top: 20px;
			padding: 2px 5px;
			background-color: #FFFFFF;

			text {
				font-size: 12px;
				font-weight: bold;
			}

			.star {
				display: flex;
				flex-direction: column;
				justify-content: center;
				align-items: center;
				margin-top: 10px;
				padding: 10px 0;

				text {
					font-size: 24px;
				}
			}
		}

		.movie_msg {
			>text {
				display: inline-block;
				font-size: 16px;
				font-weight: bold;
				padding: 20px 0 10px;
			}

			view {
				font-size: 14px;
			}
		}
	}
</style>
