<template>
    <nb-container :style="styles.bgColorBlack">
        <nb-grid>
            <nb-col :style="styles.alignItemsCenter, styles.justifyContentCenter">
                <view :style="styles.justifyContentCenter, styles.height50">
                    <text :style="styles.fontFamilyCatCafe, styles.colorWhite, styles.fontSize50">Chatify</text>
                </view>
                <view :style="styles.mt50">
                    <view>
                        <nb-button   :style="styles.bgColorWhite, styles.justifyContentCenter, styles.width250" large rounded :on-press="handleGoogleLogin">
                            <nb-icon name="logo-google" :size="24" :style="{ color: '#000000' }" />
                            <text :style="styles.fontFamilyCatCafe, styles.mt1">LOG IN WITH GOOGLE</text>
                        </nb-button>
                    </view>
                    <view :style="styles.mt7">
                        <nb-button   :style="styles.bgColorBlueFB, styles.justifyContentCenter, styles.width250" large rounded :on-press="handleFacebookLogin">
                            <nb-icon name="logo-facebook" :size="24" :style="{ color: '#FFFFFF' }" />
                            <text :style="styles.fontFamilyCatCafe, styles.colorWhite, styles.mt1">LOG IN WITH FACEBOOK</text>
                        </nb-button>
                    </view>
                </view>
            </nb-col>
        </nb-grid>
    </nb-container>
</template>

<script>
import { MainStyleSheet } from '../shared/stylesheet';
import * as firebase from 'firebase'
import * as Facebook from 'expo-facebook';
export default {
    props: ['navigation'],
    data() {
        return {
            styles: MainStyleSheet,
            didError: false
        }
    },
    mounted(){
      // this.checkIfLoggedIn()
    },
     methods: {
        // checkIfLoggedIn(){
        //     firebase.auth().onAuthStateChanged(user => {
        //         if(user){
        //             this.navigation.navigate('Profile')
        //         } else {
        //             this.navigation.navigate('Login')
        //         }
        //     })
        // },
         async handleGoogleLogin(){

         },
         async handleFacebookLogin() {
             await Facebook.initializeAsync({
                 appId: '369452944184064',
                 appName: 'chatify'
             });

             const { type, token } = await Facebook.logInWithReadPermissionsAsync(
                 { permissions: ['public_profile'] }
             );

             if (type === 'success') {
                 // Build Firebase credential with the Facebook access token.
                 const credential = firebase.auth.FacebookAuthProvider.credential(token);
                 console.log(credential)

                 // Sign in with credential from the Facebook user.
                 firebase.auth().signInWithCredential(credential).then(success => {
                     this.navigation.navigate('Profile')
                     console.log(success)
                 }).catch((error) => {
                     console.log(error)
                 });
             }
         }
     }
}
</script>
