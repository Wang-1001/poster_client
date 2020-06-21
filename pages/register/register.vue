<template>
	<view>
		<view class="title">心情海报</view>
		<view class="box">
			<view class="box-lable">注册</view>
			<form>
				<input class="text-input" v-model="loginName" type="text" name="username" placeholder="用户名" />
				<input class="text-input" v-model="nickName" type="text" name="username" placeholder="昵称" />
				<input class="text-input" v-model="password" type="password" name="pwd" placeholder="密码" />
				<input class="text-input" v-model="repeatPassword" type="password" name="pwds" placeholder="确认密码" />
				
				<button class="register" @click="doRegister">注册</button>
				
				<view class="small">未未注册手机验证码后自动登录，注册即代表同意《注册协议》《隐私保护指引》</view>
			</form>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				loginName:'',
				nickName:'',
				password:'',
				repeatPassword:''
			}
		},
		methods: {
			doRegister(){
				//判断填入数据是否为空
				if(this.loginName.length == 0 || this.nickName.length == 0 
				|| this.password.length == 0 || this.repeatPassword.length == 0){
					uni.showToast({
						title:'请输入完整的注册信息',
						icon:'none'
					});
					return;
				}
				//判断两次密码是否一致
				if(this.password != this.repeatPassword){
					uni.showToast({
						title:'两次输入的密码不一致',
						icon:'none'
					});
					return;
				}
				//注册请求
				uni.request({
					url:'http://localhost:8080/api/register',
					method:'POST',
					data:{
						lastName: this.nickName,
						login: this.loginName,
						password: this.password
					},
					success:(result) => {
						console.log("register doRegister result", result);
						//注册成功
						if(result.statusCode == 201){
							uni.showToast({
								title:'注册成功',
							});
							
							//页面延迟
							setTimeout(() =>{
								//页面返回
								uni.navigateBack();
								// //关闭当前页面，跳转到登录页
								// uni.redirectTo({
								// 	url:'../login/login'
								// })
							},1500);
						}else if(result.statusCode == 400){
							if(result.data.errorKey == "userexists"){
								uni.showToast({
									title:'用户名已被注册',
									icon:'none'
								});
							}
						}
					}
				})
			}
		}
	}
</script>

<style>

</style>
