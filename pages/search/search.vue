<template>
	<view>
		<!-- 搜索 -->
		<view class="search">
			<input type="text" placeholder="搜索" placeholder-style="font-size:14px" v-model="search_input" @confirm="search" />
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
				search_input:"",
				url:"http://t.yushu.im/v2/movie/search?apikey=0df993c66c0c636e29ecbb5344252a4a&q=",
				datalist:[]
			}
		},
		methods: {
			/* 搜索 */
			search() {
				uni.request({
					url: this.url+this.search_input,
					success: (res) => {
						this.datalist = res.data.subjects
					}
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
	.search {
		padding: 10px;
		background-color: #43BD56;

		input {
			padding: 3px;
			border-radius: 5px;
			padding-left: 10px;
			background-color: #FFFFFF;
			border: none;
		}

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
