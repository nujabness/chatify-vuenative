<template>
    <nb-container :style="styles.bgColorBlack">
        <nb-grid>
            <nb-col :style="styles.alignItemsStart, styles.mt60">
                <nb-row>
                   <text>
                       wdsfdsqfqsdfdsqf
                   </text>
                </nb-row>
            </nb-col>
        </nb-grid>
    </nb-container>
</template>

<script>
import { Dimensions } from "react-native";
import { MainStyleSheet } from '../shared/stylesheet'
import Store from "../store/store"
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

    }
}
</script>
