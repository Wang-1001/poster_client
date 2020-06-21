<template>
	<view class="content">
		<!-- <view class="text-area" v-if="loginUser != null">
			<text class="title">登录用户名：{{loginUser.login}}</text>
		</view>
		<view class="text-area">
			<text class="title">当前登录昵称：{{(loginUser != null) ? loginUser.lastName : ""}}</text>
		</view> -->
		<view class="typr-bar">
			<view class="type-item" :class="{active:navItem.id == currentType}" v-for="navItem in navList" v-bind:key="navItem.id" @click="typeClick(navItem)">{{navItem.billTypeName}}</view>
		</view>
		<view class="search-icon" >
			<icon type="search" color="#007af9" size="26"></icon>
		</view>
		<view class="search-bar" >
			<input type="text" v-model="keyWord" placeholder="占个剪辑" />
			<icon type="search" color="#007af9" size="26"></icon>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				title: 'Hello',
				loginUser:null,
				navList:[],
				currentType:0,
				keyWord:''
			}
		},
		onLoad() {
			this.getNavList();
			this.getAccount();
		},
		methods: {
			//调用当前登录账户信息
			getAccount(){
				uni.request({
					url:'http://localhost:8080/api/account',
					method:'GET',
					//取本地缓存token
					header:{
						Authorization:'Bearer ' + uni.getStorageSync("token").id_token
					},
					success:(result) =>{
						console.log("index界面的请求返回了")
						console.log("index onLoad result:",result);
						if(result.statusCode == 401){
							uni.showToast({
								title:'您还没有登录',
								icon:'none'
							});
							return;
						}else if(result.statusCode == 200){//返回成功
							this.loginUser = result.data;
						}
					}
				})
			},
			//获取海报分类
			getNavList(){
				uni.request({
					url:'http://localhost:8080/api/bill-type/all/jpa',
					method:'GET',
					success: (result) => {
						console.log("getNavList result",result);
						if(result.statusCode == 200){
							this.navList = result.data;
							//数据头部添加全部分类
							var allItem ={
								id:0,
								billTypeName:'全部',
								billTypeSort:0
							};
							this.navList.unshift(allItem);/* 头部添加 */
							/* this.navList.push(allItem);尾部添加 */
						}
					}
				})
			},
			//分类点击
			typeClick(navItem){
				this.currentType = navItem.id;
			}
		}
	}
</script>

<style>
	/* .content {
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
	}

	.logo {
		height: 200rpx;
		width: 200rpx;
		margin-top: 200rpx;
		margin-left: auto;
		margin-right: auto;
		margin-bottom: 50rpx;
	}

	.text-area {
		display: flex;
		justify-content: center;
	}

	.title {
		font-size: 36rpx;
		color: #8f8f94;
	} */
	
	.typr-bar{
		position: fixed;
		/* top: 0; */
		display: flex;
		flex-direction: row;
		width: calc(750rpx - 57px);
		overflow-x: scroll;
		flex-wrap: nowrap;
		flex-basis: 200rpx;
		white-space: normal;
	}
	.type-item{
		margin-left: 10rpx;
	}
	.active{
		border-bottom: 4px solid #007AFF;
	}
	.search-bar input{
		height: 42px;
		width: 600rpx;
		padding: 0 20rpx;
		border-bottom: 1px solid #4CD964;
	}
</style>
