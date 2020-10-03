<template>
  <style-wrapper>
    <view class="container">
      <app-loading v-if="!isAppReady"> </app-loading>
      <app v-if="isAppReady"></app>
    </view>
  </style-wrapper>
</template>

<script>
import Vue from "vue-native-core";
import { VueNativeBase } from "native-base";
import { AppLoading } from "expo/build/Expo";
import {StyleWrapper} from "../components/StyleWrapper";
import * as Font from "expo-font";
import App from "../App.vue";
import firebase from 'firebase'
import { FIREBASE_CONFIG } from "../shared/constants";

Vue.use(VueNativeBase);

export default {
  components: { App, AppLoading, StyleWrapper },
  data() {
    return {
      isAppReady: false
    };
  },
  created() {
    this.loadFonts();
    this.initFirebase()
  },
  methods: {
    initFirebase(){
      firebase.initializeApp(FIREBASE_CONFIG )
    },
    async loadFonts() {
      try {
        this.isAppReady = false;
        await Font.loadAsync({
          CatCafe: require("../../assets/fonts/CatCafe.ttf"),
          Roboto: require("../../node_modules/native-base/Fonts/Roboto.ttf"),
          Roboto_medium: require("../../node_modules/native-base/Fonts/Roboto_medium.ttf"),
          Ionicons: require("../../node_modules/@expo/vector-icons/build/vendor/react-native-vector-icons/Fonts/Ionicons.ttf")
        });
        this.isAppReady = true;
      } catch (error) {
        console.log("some error occured", error);
        this.isAppReady = true;
      }
    }
  }
};
</script>
<style>
  .container {
    flex: 1;
  }
</style>
