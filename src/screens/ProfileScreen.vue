<template>
    <nb-container :style="styles.bgColorBlack">
        <nb-grid>
            <nb-col :style="styles.alignItemsStart, styles.mt60">
                <nb-row>
                   <text :style="styles.colorWhite, styles.fontFamilyCatCafe">
                       PROFILE
                   </text>
                    <nb-button :style="styles.bgColorWhite, styles.justifyContentCenter, styles.width250" large rounded :on-press="handleLogOut">
                        <text :style="styles.fontFamilyCatCafe, styles.mt1">LOG OUT</text>
                    </nb-button>
                </nb-row>
            </nb-col>
        </nb-grid>
    </nb-container>
</template>

<script>
import { Dimensions } from "react-native";
import { MainStyleSheet } from '../shared/stylesheet'
import Store from "../store/store"
import firebase from 'firebase'
const deviceWidth = Dimensions.get("window").width;
const deviceHeight = Dimensions.get("window").height;export default {
    props: ['navigation'],
        data() {
            return {
                styles: MainStyleSheet,
                recentlyPlayedTracks: [],
                topArtists: [],
                topTracks: [],
                stylesObj: {
                    cardItemImage: {
                        resizeMode: "cover",
                        height: deviceHeight / 4,
                        width: deviceWidth
                    }
                }
            }
        },
    mounted(){
    },
    computed: {
        user() {
            return Store.state.user
        }
    },
    methods: {
        handleLogOut() {
            firebase.auth().signOut()
        }
    }
}
</script>
