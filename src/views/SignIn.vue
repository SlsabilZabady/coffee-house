<template>
    <div class="sign-in">
        <div class="wrapper">
            <div class="row">
                
                <div class="col-md-6 right-side">
                    <section class="container-fluid">
                        <section class="row ">
                            <section class="col-12 col-sm-6 col-md-3">
                                <div
                                    class="form-container"
                                    
                                >
                                    
                                    <div class="form-group">
                                        <vue-fontawesome
                                        icon="envelope"
                                        size="1"
                                    ></vue-fontawesome
                                    ><label for="exampleInputEmail1"
                                        >Email address</label
                                    >
                                    <input
                                        type="email"
                                        v-model="email"
                                        class="form-control"
                                        id="exampleInputEmail1"
                                        aria-describedby="emailHelp"
                                       
                                    />
                                    
                                    </div>

                                    <div class="form-group">
                                        <i class="fa fa-lock"></i
                                        ><label for="exampleInputPassword1">
                                            Password</label
                                        >
                                        <input
                                            type="password"
                                            v-model="password"
                                            class="form-control password"
                                            id="password"
                                            
                                        />
                                    </div>
                                    <div class="form-group form-check">
                                        <input
                                            type="checkbox"
                                            class="form-check-input"
                                            id="exampleCheck1"
                                            @click="showPasswordBtn"
                                            v-model="showPassword"
                                        />
                                        <label
                                            class="form-check-label"
                                            for="showPassword"
                                            >Show Password</label
                                        >
                                    </div>
                                    <button
                                       
                                        class="btn btn-primary btn-block"
                                        @click="logIn"
                                    >
                                        Sign in
                                    </button>
                                    <div class="noaccount">
                                        Don't have an account?<a href="/signup"
                                            >Sign Up</a
                                        >
                                    </div>
                                </div>
                            </section>
                        </section>
                    </section>
                </div>
                <div class="col-md-6 left-side">
                    <div class="feature-box">
                        <img
                            src="../assets/66783.png"
                            class="logo justify-content-center"
                        />
                        <p>WELCOME BACK !</p>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>


<script>
import {firebaseApp } from '../firebase';
//import db from 'firebase';
import { required, email, minLength } from "vuelidate/lib/validators";
export default {
    name: "signIn",
    data() {
        return {
            
            name: null,
            email: null,
            password: null,
            showPassword: false,
            submitStatus: null
        };
    },
    validations: {
        username: {
            required,
            isUnique(value) {
                if (value === "") return true;
                return new Promise(resolve => {
                    setTimeout(() => {
                        resolve(
                            typeof value === "string" && value.length % 2 !== 0
                        );
                    }, 350 + Math.random() * 300);
                });
            }
        },
        email: {
            required,
            email,
            isUnique(value) {
                if (value === "") return true;
                var email_regex = /^(([^<>()[\]\\.,;:\s@"]+(\.[^<>()[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/;
                return new Promise(resolve => {
                    setTimeout(() => {
                        resolve(email_regex.test(value));
                    }, 350 + Math.random() * 300);
                });
            }
        },
        password: {
            required,
            minLength: minLength(8)
        }
    },
    methods: {
        showPasswordBtn() {
            var show = document.getElementById("password");
            if (this.showPassword == false) {
                this.showPassword = true;
                show.type = "text";
            } else {
                this.showPassword = false;
                show.type = "password";
            }
        },
        logIn() {
            firebaseApp
                .auth()
                .signInWithEmailAndPassword(this.email, this.password)
                .then (()=>{
                    this.$router.replace("account");
                })
                .catch((error)=> {
                    // Handle Errors here.
                    var errorCode = error.code;
                    var errorMessage = error.message;
                    if (errorCode === "auth/wrong-password") {
                        alert("Wrong password.");
                    } else {
                        alert(errorMessage);
                    }
                    console.log(error);
                    console.log('welcome');
                    
                });
        }
    }
};
</script>
<style scoped>
.wrapper {
    position: relative;
    margin: 10px 100px;
    border-radius: 10px;
    padding-top: 50px;
    padding-bottom: 50px;
    box-shadow: 0 0 20px 2px rgba(247, 197, 168, 0.6);
}
.feature-box {
    padding: 30px;
}
.left-side {
    border-left: 1px solid gray;
}
.left-side img {
    position: absolute;
    top: 5%;
    width: 400;
    height: 375px;
    align-items: center;

    margin-bottom: 20px;
    margin-left: 100px;
}
.left-side p {
    margin-top: 75%;
    text-align: center;
    margin-bottom: 10px;
    font-size: 30px;
    margin-left: 40px;
}
.right-side {
    padding: 0 50px;
}
.form-container {
    position: absolute;
    margin-bottom: 20px;
    margin-top: 60px;
    align-items: center;
    border-radius: 10px;
    padding: 40px;
    box-shadow: 0 0 20px 2px rgba(206, 137, 57, 0.2);
    margin-left: 30px;
}
 input[type="email"],
input[type="password"],
input[type="text"] {
    width: 300px;
    border: none;
    border-bottom: 1px solid gray;
    border-radius: 0;
}
 .btn-block {
    background: #865934;
    border: none;
    border-radius: 25px;
    padding: 10px 0;
    margin: 10px 0;
}
 .btn-block:hover {
    background: white;
    color: #865934;
    border: 1px solid #865934;
}
.noaccount {
    text-align: center;
    font-size: 12px;
    margin-bottom: 20px;
}
label {
    margin-left: 10px;
    
}

.valid-feedbak,
.invalid-feedbak {
    color: red;
    font-size: 10px;
    padding: 5px;
}

@media only screen and (min-width: 341px) {
    .logo p {
        top: 20vh;
        margin-top: 50px;
        font-size: 20px;
    }
   
}
</style>
