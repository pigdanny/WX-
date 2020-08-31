<template>
	<view>
		<!-- 登录 -->
		<view class="btn">
			<text>登录</text>
		</view>
		<!-- 我的书影音 -->
		<view class="book">
			<view class="title">
				<text>我的书影音</text>
				<text>登录查看></text>
			</view>
			<view v-for="(item,index) in dataList" :key="index" class="contents">
				<view class="left">
					<text>{{item.name}}</text>
					<text>0{{item.action}}</text>
				</view>
				<view class="right">
					<text>{{item.open}}</text>
					<text>立即开启></text>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				dataList: [{
					name: "观影分析",
					action: "看过",
					open: "标记10部影片开启观影分析",
				}, {
					name: "读书分析",
					action: "读过",
					open: "标记10本书开启读书分析",
				}, {
					name: "音乐分析",
					action: "听过",
					open: "标记10首歌开启音乐分析",
				}, ]
			}
		},
		onPullDownRefresh() {
			uni.stopPullDownRefresh()
		},
		methods: {

		}
	}
</script>

<style lang="less" scoped>
	.btn {
		height: 200px;
		background-color: #43BD56;
		display: flex;
		justify-content: center;
		align-items: center;

		text {
			width: 150px;
			height: 40px;
			line-height: 40px;
			color: #5A6F5D;
			background-color: #FFFFFF;
			border-radius: 40px;
			text-align: center;
			font-weight: bold;
		}
	}
	.book {
		display: flex;
		flex-direction: column;
		padding: 10px;

		.title {
			display: flex;
			justify-content: space-between;
			align-items: center;

			text {
				&:first-child {
					font-size: 14px;
					font-weight: bold;
				}

				&:last-child {
					font-size: 12px;
					color: #BCBCBC;
				}

			}
		}
		.contents{
			display: flex;
			justify-content: space-between;
			font-size: 12px;
			border-bottom: 1px solid #f5f5f5;
			.left{
				padding: 10px 0;
				text{
					display: flex;
					flex-direction: column;
					&:last-child{
						color: #BCBCBC;
						line-height: 40px;
					}
				}
			}
			.right{
				align-self: flex-end;
				padding-bottom: 10px;
				
				text{
					&:first-child{
						display: inline-block;
						width: 80px;
						color: #bcbcbc;
						background-color:#FDFDFD ;
					}&:last-child{
						display: inline-block;
						width: 60px;
						padding: 5px;
						background-color:#F5F5F5;
					}
				}
			}
		}
	}
</style>
