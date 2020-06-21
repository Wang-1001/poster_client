<template>
	<view class="page">
		<view class="title">心情海报</view>
		<view class="box">
			<view class="box-lable">账号密码登录</view>
			<form>
				<input class="text-input" type="text" name="username" placeholder="手机号或邮箱" v-model="loginName"/>
				<input class="text-input" type="password" name="pwd" placeholder="密码" v-model="password"/>
				<view class="login-link">
					<view @click="gotoRegister">注册用户</view>
				</view>
				<button class="login" @click="doLogin">登录</button>
				<view class="small">未注册手机验证码后自动登录，注册即代表同意《注册协议》《隐私保护指引》 </view>
			</form>
		</view>
	</view>
</template>

<script>
	export default {
		//数据定义部分
		data() {
			return {
				loginName:'',
				password:''
			}
		},
		//页面逻辑定义
		methods: {
			//登录
			doLogin(){
				console.log("登录",this.loginName,this.password);
				//验证用户名或密码是否为空
				if(this.loginName.length == 0 || this.password.length ==0){
					//提示输入用户名密码
					uni.showToast({
						title:'请输入用户名或密码',
						icon:'none'
					});
					return;
				}
				//发送登录请求
				uni.request({
					url:'http://localhost:8080/api/authenticate',
					method:'POST',
					data:{
						password: this.password,
						rememberMe: true,
						username: this.loginName
					},
					success:function(request){
						console.log("响应成功",request);
						if(request.statusCode == 200){
							//登录用户token存入本地缓存
							uni.setStorageSync("token",request.data)
							//登录成提示
							uni.showToast({
								title:'登录成功'
							});
							//页面延迟
							setTimeout(function(){
								//主界面跳转
								uni.navigateTo({
									url:'../index/index'
								})
							},1500)
							
						}else{
							//登录失败提示
							uni.showToast({
								title:'用户名或密码错误！',
								icon:'none'
							});
						}
					}
				})
			},
			//注册界面跳转
			gotoRegister(){
				uni.navigateTo({
					url:'../register/register'
				})
			},
			
		},
		//生命周期定义
		onLoad(){
			
		},
		onShow() {
			
		}
	}
</script>

<style>

</style>
