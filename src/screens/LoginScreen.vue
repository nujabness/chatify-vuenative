<template>
    <nb-container :style="styles.bgColorBlack">
        <nb-grid>
            <nb-col :style="styles.alignItemsCenter, styles.justifyContentCenter">
                <view :style="styles.justifyContentCenter, styles.height50">
                    <text :style="styles.fontFamilyCatCafe, styles.colorWhite, styles.fontSize50">Chatify</text>
                </view>
                <view :style="styles.width300, styles.justifyContentCenter, styles.mt50">
                    <view>
                        <nb-form>
                            <nb-item stackedLabel>
                                <nb-label :style="styles.colorWhite">Email</nb-label>
                                <nb-input :style="styles.colorWhite" v-model="email"/>
                            </nb-item>

                            <nb-item stackedLabel>
                                <nb-label :style="styles.colorWhite">Password</nb-label>
                                <nb-input :secureTextEntry="true"
                                          :style="styles.colorWhite" v-model="password"/>
                            </nb-item>
                        </nb-form>
                    </view>
                </view>
               <nb-row :size="0.16" :style="styles.mt5">
                   <nb-col :style="styles.alignItemsEnd">
                       <view :style="styles.mr5">
                           <nb-button :style="styles.bgColorWhite, styles.justifyContentCenter, styles.width140" rounded :on-press="handleLogin">
                               <text :style="styles.fontFamilyCatCafe, styles.mt1">LOG IN</text>
                           </nb-button>
                       </view>
                   </nb-col>
                   <nb-col :style="styles.alignItemsStart">
                       <view :style="styles.ml5">
                           <nb-button :style="styles.bgColorWhite, styles.justifyContentCenter, styles.width140" rounded :on-press="handleRegister">
                               <text :style="styles.fontFamilyCatCafe, styles.mt1">REGISTER</text>
                           </nb-button>
                       </view>
                   </nb-col>
               </nb-row>
               <view v-if="isIos">
                   <nb-button :style="styles.bgColorWhite, styles.justifyContentCenter, styles.width300" rounded :on-press="handleGoogleLogin">
                       <nb-icon name="logo-google" :size="24" :style="{ color: '#000000' }" />
                       <text :style="styles.fontFamilyCatCafe, styles.mt1">LOG IN WITH GOOGLE</text>
                   </nb-button>
               </view>
               <view v-if="isIos" :style="styles.mt3">
                   <nb-button :style="styles.bgColorBlueFB, styles.justifyContentCenter, styles.width300" rounded :on-press="handleFacebookLogin">
                       <nb-icon name="logo-facebook" :size="24" :style="{ color: '#FFFFFF' }" />
                       <text :style="styles.fontFamilyCatCafe, styles.colorWhite, styles.mt1">LOG IN WITH FACEBOOK</text>
                   </nb-button>
               </view>
           </nb-col>
        </nb-grid>
    </nb-container>
</template>

<script>
import { MainStyleSheet } from '../shared/stylesheet';
import firebase from 'firebase'
import * as Facebook from 'expo-facebook';
import { Platform } from 'react-native';
import { FACEBOOK_APP_ID } from '../shared/constants'
export default {
    props: ['navigation'],
    data() {
        return {
            styles: MainStyleSheet,
            didError: false,
            email: "",
            password: "",

        }
    },
    computed:{
      isIos(){
          return Platform.OS != 'ios'
      }
    },
    mounted(){
      this.checkIfLoggedIn()
    },
     methods: {
        checkIfLoggedIn(){
            firebase.auth().onAuthStateChanged(user => {
                if(user){
                    this.navigation.navigate('Profile')
                } else {
                    this.navigation.navigate('Login')
                }
            })
        },

         handleLogin() {
             firebase.auth().signInWithEmailAndPassword(this.email, this.password).then(response => {
                 console.log(response)
                 this.navigation.navigate('Profile')
             }).catch(error => {
                 console.log(error)
             })
         },
         handleRegister() {
            firebase.auth().createUserWithEmailAndPassword(this.email, this.password)
         },
         async handleGoogleLogin(){

         },
         async handleFacebookLogin() {
             await Facebook.initializeAsync({FACEBOOK_APP_ID});
             const { type, token } = await Facebook.logInWithReadPermissionsAsync({ permissions: ['public_profile', 'email'] });
             if (type === 'success') {
                 const credential = firebase.auth.FacebookAuthProvider.credential(token);
                 firebase.auth().signInWithCredential(credential).then(success => {
                     console.log(success)
                     this.navigation.navigate('Profile')
                 }).catch((error) => {
                     console.log(error)
                 });
             }
         },
     }
}
</script>
