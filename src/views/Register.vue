<template>

    <div id="register">
        <div id="register-page" class="row">
            <div class="col s3 z-depth-4 card-panel">
                <form class="register-form">
                    <div class="row">
                    <div class="input-field col s12 center">
                        <img src="" alt="" class="circle responsive-img valign profile-image-register">
                        <p class="center register-form-text">TODO FANCY</p> <br>
                        <p class="center register-form-text">Create Your Account</p>
                    </div>
                    </div>
                    <div class="row margin">
                        <div class="input-field col s12">
                            <i class="material-icons prefix">person</i>
                            <input id="username" type="text" v-model="username">
                            <label for="username" class="center-align">username</label>
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
                        <button id="createAccount" class="btn waves-effect waves-light col s12" v-on:click="createAccount()">CREATE ACCOUNT</button>
                    </div>
                </div>
                
            </div>
        </div>

    </div>
</template>

<script>
import axios from "axios"


export default {
    created() {
        if(localStorage.hasOwnProperty("token")) {
            this.$router.push("todo")
        }
    },

    data() {
        return {
            username:"",
            password:"",
            email:""
        }
    },
    methods:{
        createAccount() {
            axios({
                method:"post",
                url:"http://localhost:3000/users/signup",
                data: {
                    username: this.username,
                    password: this.password,
                    email: this.email
                }
            })
            .then(response=>{
                console.log(response)
                this.$router.push("/")
            })
            .catch(err=>{
                console.log(err.response.data)
            })
        }
    }
}
</script>

