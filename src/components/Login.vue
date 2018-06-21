<template>
    <div id="login">
                <!-- Start Page Loading -->
        <!-- <div id="fb-root"></div> -->
        <!-- <div id="loader-wrapper">
            <div id="loader"></div>        
            <div class="loader-section section-left"></div>
            <div class="loader-section section-right"></div>
        </div> -->
        <!-- End Page Loading -->
        <div id="login-page" class="row">
            <div class="col s3 z-depth-4 card-panel">
                <form class="login-form">
                    <div class="row">
                        <div class="input-field col s12 center">
                            <img src="" alt="" class="circle responsive-img valign profile-image-login">
                            <p class="center login-form-text">TODO FANCY</p>
                        </div>
                    </div>
                    <div class="row margin">
                        <div class="input-field col s12">
                            <i class="material-icons prefix">email</i>
                            <input id="email" type="text" v-model="email">
                            <label for="email" class="center-align">Email</label>
                        </div>
                    </div>
                    <div class="row margin">
                        <div class="input-field col s12">
                            <i class="material-icons prefix">lock</i>
                            <input id="password" type="password" v-model="password">
                            <label for="password">Password</label>
                        </div>
                    </div>
                
                </form>
                <div class="row">
                    <div class="col s12">
                        <button id="loginButton" class="btn waves-effect waves-light col s12" v-on:click="signIn()">Login</button>
                    </div>
                </div>
                <div class="row">
                    <div class="col s12">
                        <button id="fbLoginButton" onlogin="checkLoginState()" class="btn waves-effect waves-light col s12" v-on:click="signInFb()">Login with Facebook</button>
                    </div>
                    
                </div>
                <div class="row">
                    <div class="input-field col s6 m6 l6">
                        <p class="margin medium-small"><a href v-on:click="registerLink()">Register Now!</a></p>
                    </div>
                    <div class="input-field col s6 m6 l6">
                        <p class="margin right-align medium-small"><a href="page-forgot-password.html">Forgot password ?</a></p>
                    </div>          
                </div>
            </div>
        </div>

    </div>
</template>

<script>
import axios from "axios"

export default {
    data() {
        return {
            email:"",
            password:""
        }
        
    },
    created: function() {
        if(localStorage.hasOwnProperty("token")) {
            this.$router.push("todo")
        }else{
            (function(d, s, id) {
                var js, fjs = d.getElementsByTagName(s)[0];
                if (d.getElementById(id)) return;
                js = d.createElement(s); js.id = id;
                js.src = "https://connect.facebook.net/en_US/sdk.js";
                fjs.parentNode.insertBefore(js, fjs);
            }(document, 'script', 'facebook-jssdk'));

            window.fbAsyncInit = function() {
                console.log("msk")
                
                FB.init({
                appId      : '262897097612945',
                cookie     : true,                       
                xfbml      : true,  
                version    : 'v2.8' 
                });
            };
        }
        

    },
    methods:{

        signIn() {
            axios.post('http://localhost:3000/users/signin',{
                email:this.email,
                password:this.password
            })
            .then(({data})=>{
                console.log("masuk2")
                if(data) {

                    localStorage.setItem("token",data.token)
                    this.$router.push("todo")
                }
                console.log(data,"masuk");
            })
            .catch(err=>{
                console.log("error")
                console.log(err.response.data)
            })
        },
        signInFb() {
            console.log("maskk")
            let self= this                      
           FB.login(function(response) {
               if(response.status === "connected") {
                   FB.api('/me',{fields:["email","first_name"]},function(dataUser) {
                       console.log(dataUser)
                       axios({
                           method:"post",
                           url:'http://localhost:3000/users/signin',
                           data: {
                               email: dataUser.email,
                               username: dataUser.first_name,
                               userId: dataUser.id
                           }
                       })
                       .then(({data})=>{
                           localStorage.setItem("token",data.token)
                            self.$router.push("todo")
                       })
                   })
               }
            })
        },
        registerLink() {
            this.$router.push("register")
        }

        
    }    



}


</script>



<style scoped >

/* html,
html {
    display: table;
    margin: auto;
} */
/* body {
    height: 100%;
    display: table-cell;
    vertical-align: middle;
    background-color: #84ffff
} */


#login-page {
    text-align: center
}

#loginButton {
    background-color:#e57373
}
</style>

