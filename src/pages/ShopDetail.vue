<template>
<div class="shop_list">
    <div class="shop_header">
        <div class="back_icon"><span @click='$router.go(-1)'><</span></div>
        <div class="shop_info">
            <div class="shop_logo"><img :src="storeIn.image_path | imgForm" alt=""></div>
            <div class="shop_i">
                <p>{{storeIn.name}}</p>
                <p><span>商家配送</span>/<span>{{storeIn.order_lead_time}}分钟送达</span>/<span>配送费￥{{storeIn.float_delivery_fee}}</span></p>
                <p>公告：{{storeIn.description}}</p>
            </div>
            <div class="shop_detail">
                <span>&gt;</span>
            </div>
        </div>
        <ul class='shop_ad_box'>
            <li class="shop_ad" v-for='item in storeIn.activities' :key="item">
                <span>{{item.icon_name}}</span>
                <span>{{item.tips}}</span>
                <span>{{storeIn.activities.length}}个活动</span>
            </li>
        </ul>

    </div>
    <div class="shop_cont">
        <div class="shop_comment ">
            <span @click='currentView="shangjia";shangpin_pingjia=1' :class="{typeactive:shangpin_pingjia==1}">商品</span>
            <span @click='currentView="pingjia";shangpin_pingjia=2' :class="{typeactive:shangpin_pingjia==2}">评价</span></div>
        <component :is='currentView' :shop_id='params.id' :longitude='params.longitude' :latitude='params.latitude' :psf='storeIn.float_delivery_fee'></component>
    </div>
</div>
</template>

<script>
import imgFormat from '../utils/utils.js'
// 二级路由组件
import shangjiaxinxiShop from '../components/shangjiaxinxi_shop'
import shangjiaxinxiPingjia from '../components/shangjiaxinxi_pingjia'
import store from '../vuex/store'

export default {
    data() {
        return {
            shangpin_pingjia: 1,
            storeIn: {}, //商家信息
            currentView: 'shangjia', //商家&评价 组件初始化
            params: {}
        }
    },
    methods: {
        // 商家基本信息
        getShopInfo() {
            this.$http.get(`https://www.ele.me/restapi/shopping/restaurant/${this.params.id}?extras%5B%5D=activities&extras%5B%5D=license&extras%5B%5D=identification&extras%5B%5D=albums&extras%5B%5D=flavors&latitude=${this.params.latitude}&longitude=${this.params.longitude}`).then(res => this.storeIn = res.data);
        }
    },
    created() {
        this.params = this.$route.params;
    },
    mounted() {
        this.getShopInfo();
    },
    components: {
        shangjia: shangjiaxinxiShop,
        pingjia: shangjiaxinxiPingjia
    },
    filters: {
        imgForm(i) {
            return imgFormat(i);
        }
    }
}
</script>

<style scoped>
.zw{
    width:100%;
    height:3.2rem;
}
.shop_list {
    display: relative;
}

.typeactive{
    color:blue;
    border-bottom:2px solid #3190e8;
    z-index:10;
}
/*商家信息*/

.shop_header {
    color: #fff;
    background: url(../imgs/others/header_bg.png) no-repeat;
    background-size: 100%;
}

.back_icon {
    height: 2rem;
    line-height: 2rem;
}

.back_icon span {
    margin-left: 1rem;
    font-size: 1.2rem;
}

.shop_info {
    position: relative;
}

.shop_info .shop_logo {
    float: left;
    width: 25%;
}

.shop_info .shop_logo img {
    display: block;
    width: 80%;
    margin: 0px auto;
}

.shop_info .shop_i {
    float: left;
    width: 75%;
    line-height: 1.5rem;
}

.shop_info .shop_i p:nth-child(1) {
    font-size: 1.3rem;
    font-weight: bold;
}

.shop_info .shop_i p:nth-child(3) {
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
}

.shop_detail {
    position: absolute;
    top: 50%;
    transform: translateY(-50%);
    right: 1rem;
}

.shop_ad_box {
    height: 2rem;
    line-height: 2rem;
    overflow-y: hidden;
}

.shop_ad {
    font-size: .8rem;
    height: 2rem;
    line-height: 2rem;
}

.shop_ad span {
    box-sizing: border-box;
    float: left;
}

.shop_ad span:nth-child(1) {
    background-color: green;
    width: 10%;
    text-align: center;
    vertical-align: top;
}

.shop_ad span:nth-child(2) {
    width: 70%;
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
}

.shop_ad span:nth-child(3) {
    width: 20%;
    text-align: center;
    vertical-align: top;
    /* float: right; */
    /* margin-right: 1rem; */
}

.shop_comment {
    height: 3rem;
    font-size: 0;
    line-height: 3rem;
}

.shop_comment span {
    width: 50%;
    font-size: 1.3rem;
    display: inline-block;
    text-align: center;
}


/*清除浮动*/

.shop_info:after,
.list_right_con:after,
.zhpj:after,
.pJ:after {
    content: "";
    display: block;
    clear: both;
}
</style>