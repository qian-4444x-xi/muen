<template>
  <div class='login-box'>
     <div class="title">
         欢迎来到沐恩之家
     </div>
     <div class="inner">
         <div class="user block">
             <input type="text" placeholder="请输入您的用户名" v-model="user">
         </div>
         <div class="pwd block">
             <input type="password" placeholder="请输入密码" v-model="pwd">
         </div>
         <div class="register">
             <span @click="gotoRegister">立即注册</span>
             <span>忘记密码</span>
         </div>
         <div class="login block">
             <span @click="gotoHome">登录</span>
         </div>
     </div>
  </div>
</template>

<script>
import {login} from "@/api/api.js"
export default {
   data(){
       return {
           user:"",
           pwd:""
       }
   },
   methods:{
    //    点登录按钮
     async gotoHome(){
            const res=await login({userName:this.user,password:this.pwd})
            // console.log(res)
                if(res.code===1){//判断成功
                              window.localStorage.setItem("userId",res.userId)

                    window.localStorage.setItem('token',res.token);// 本地存储
                    let redirect=this.$route.query.redirect;
                    // console.log(redirect)
                    if(redirect){
                         this.$router.push({path:redirect}) // 跳到要去的页面
                    }else{
                        this.$router.push({path:"/home"})
                    }
                }else{//否则跳到注册页面
                    this.$router.push({path:"/register",query:{redirect:this.$route.query.redirect}})
                }
       },
       gotoRegister(){ // 立即注册
           this.$router.push({path:"/register",query:{redirect:this.$route.query.redirect}})
       }
   }
}
</script>

<style scoped lang="scss">
.login-box{
    width: 100%;
    height: 100%;
    display: flex;
    flex-direction: column;
}
.title{
    margin-top:124px;
    width: 100%;
    padding-left: 107px;
    height: 30px;
    line-height: 30px;
    font-size: 20px;
}

.inner{
    width: 100%;
    height: 100%;
    overflow: hidden;
    margin-top: 35px;
    padding-left: 65px;
}
.inner .block{
    width: 245px;
    border:1px solid #ccc;
    margin-bottom: 17px;
    height: 36px;
    line-height: 36px;
    font-size: 12px;
    border-radius: 2px;
    overflow: hidden;
    input{
        width: 100%;
        border:none;
        outline: none;
        padding-left: 10px;
        background: none;
        ::placeholder{
            font-size: 12px;
        }
    }
}
.inner.pwd{
    margin-bottom: 5px;
}
.register{
    overflow: hidden;
    width: 245px;
    margin-bottom: 17px;
    font-size: 10px;
    display: flex;
    justify-content: space-between;
    span:last-child{
            color:gold;
    }
}
.login{
    overflow: hidden;
    text-align: center;
    width: 100%;
    height: 100%;
    background: rgba(255,255,255,1);
    span{
        width: 100%;
        height: 100%;
    }
}
</style>