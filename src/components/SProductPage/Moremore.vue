<script>
import { RouterLink } from 'vue-router';

export default{
    props: {
        nameholders: String,
        picc: String,
        coster: String,
        tag11: String,
        tag22: String,
        tag11Color: String,
        tag22Color: String,
        nameholder: String,
        Rou: String
    }
}
</script>

<template>
    <RouterLink :to="Rou" class="Rou">
        <div class="fcontainer1">
            <div class="fcontainer">
                <img :src="picc" alt="">
                <div class="namee">{{nameholders}}</div>
                <div class="cos">{{ coster }}</div>
                <div class="tag-container1">
                    <div class="tag11">{{tag11}}</div>
                    <div class="tag22">{{ tag22 }}</div>
                </div>
            </div>
        </div>
    </RouterLink>
</template>

<style scoped>
    .fcontainer img{
        width: 200px;
        height: 200px;
    }

    .tag11{
        border-radius: 1px solid v-bind(tag11Color);
        background-color: v-bind(tag11Color);
        font-size: 11px;
        /* margin-right: 10px; */
        padding-left: 10px;
        padding-right: 10px;
        border-radius: 15px;
    }

    .tag22{
        border-radius: 1px solid v-bind(tag22Color);
        background-color: v-bind(tag22Color);
        font-size: 11px;
        display: flex;
        flex-direction: row;
        align-items: center;
        justify-content: center;
        padding-right: 10px;
        padding-left: 10px;
        border-radius: 15px;
    }

    .tag-container1{
        width: auto;
        height: 20px;
        display: flex;
        flex-direction: row;
        align-items: center;
        justify-content: space-between;
        margin-top: 5px;
        gap: 15px;
    }

    .fcontainer{
        width: 200px;
        height: 320px;
        display: flex;
        flex-direction: column;
        align-items: flex-start;
        /* margin-top: 10px; */
        /* margin-left: 10px; */
        padding: 0;
        /* box-sizing: border-box; */
        /* overflow: hidden; */
        /* margin-left: 7.5px;
        margin-right: 7.5px; */
    }

    .fcontainer .namee{
        font-size: 15px;
        height: 60px;
        width: auto;
        /* text-wrap: wrap;
        word-break: normal; */
        word-wrap: break-word;
        margin-top: 5px;
    }
    
    .fcontainer .coster{
        font-size: 15px;
        font-weight: 400px;
        margin-bottom: 5px;
    }

    .Rou {
        text-decoration: none;
        color: black;
    }
</style>