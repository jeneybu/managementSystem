<template>
    <div class="Browse">
        <div class="carousel">
            <Carousel autoplay>
                <Carousel-item v-for="(item, index) in advList" :key="index">
                    <div class="demo-carousel" @click="towebsite(item.forwardUrl)">
                        <img :src="item.imageUrl" alt="">
                        <span class="imgtxt">{{item.title}}</span>
                    </div>
                </Carousel-item>
            </Carousel>
        </div>
        <div class="tagbox">
            <h3>文章分类</h3>
            <ul class="tagul">
                <li class="tagli" v-for="(item, index) in tagList" :key="index" @click="getOrderData(item.id)">
                    <img src="../assets/imgs/user.jpg" alt="">
                    <span class="tagspan">{{item.name}}</span>
                </li>
            </ul>
        </div>
        <div class="contant">
            <div class="comment">
                <h3 class="list_title">精彩文章<Icon type=" checkmark"></Icon></h3>
                <ul>
                    <li v-for="(item, index) in dataList" :key="index" @click="toDetail(item.id)">
						<div class="list_block_l"><img :src="item.thumbnailUrl" alt=""></div>
						<div class="list_block_r">
							<div class="list_block_r_info">
                                <span>{{item.title}}</span>
                            </div>
							<div class="list_block_r_text">{{item.intro}}</div>
							<div class="list_block_r_bottom">
								<div class="list_bottom_info_l">{{item.updateTime}}</div>
								<div class="list_bottom_info_r">
									<span><Icon type="ios-thumbs-up-outline"/>{{item.praiseTimes}}</span>
									<span>点赞</span>
                                </div>
							</div>
						</div>
                    </li>
                </ul>
            </div>
        </div>
    </div>
</template>

 <script>
 export default {
     name: 'BrowsePage',
     data() {
         return {
             title: '你认识萌萌的小熊猫吗',
             time: '2019-7-18',
             commentText: '',
             isShowC: false,
             pageSize: 100,
             currentPage: 1,
             dataList: [],
             advList: [],
             tagList: []

         }
     },
     created() {
         this.getOrderData('')
         this.getAdvData('')
         this.getTag()
     },
     methods: {
        //  跳转链接
        towebsite (website) {
            window.location.href = website
        },
        toDetail(id) {
            sessionStorage.setItem('titleId', id)
            this.$router.push({
                 path: '/BrowsePage',
                 query: {
                     id : id
                 }
             })
         },
         handleC () {
             this.isShowC = !this.isShowC
         },
         // 获取广告数据
        getAdvData() {
            this.$get("/front/advertisement/getList").then(res => {
                if (res.code == "SUCC") {
                var res = res.result;
                //   this.total = res.total;
                this.advList = res.data;
                } else {
                this.$Message.warning(res.message);
                }
            });
        },
         // 获取列表数据
        getOrderData(id) {
            this.$get("/front/article/getList?pageNum="+this.currentPage+"&pageSize="+ this.pageSize+"&articleCategoryId="+id).then(res => {
                if (res.code == "SUCC") {
                var res = res.result;
                //   this.total = res.total;
                this.dataList = res.data;

                } else {
                this.$Message.warning(res.message);
                }
            })
        },
         // 获取标签
        getTag () {
            this.$get("/front/article_category/getList").then(res => {
                if (res.code == "SUCC") {
                var res = res.result;
                console.log(res, 'sass')
                this.tagList = res.data;
                } else {
                this.$Message.warning(res.message);
                }
            });
         },
     },
 }
 </script>

<style scoped>
 .tagbox{
     margin: 20px 0;
     height: auto;
 }
 .tagul{
     margin-top: 5px;
     padding: 10px 5px;
     border-bottom: 1px solid #f1f1f1;
     border-top: 1px solid #f1f1f1;
     width: 100%;
     display: flex;
     justify-content: space-between;
     overflow-x:scroll
 }
.tagbox h3{
    color: #222;
    font-size: 16px;
 }
 .tagli{
     text-align: center;
     margin: 0 10px;
     font-size: 14px;
     color: brown

 }
 .tagli img{
     width: 40px;
     border-radius: 50%
 }
.Browse{
    max-width: 640px;
    height: 100%;
    margin: 0 auto;
    position: relative;
    overflow: scroll
}
.carousel .demo-carousel{
    position: relative
}
.carousel .demo-carousel img{
    width: 100%;
}
.carousel .demo-carousel .imgtxt{
    position: absolute;
    bottom: 40px;
    left: 20px;
    font-size: 26px;
    color: brown
}
.comment{
    border-bottom: 2px solid #ececec;
    padding: 0 7px
}
.comment ul li{
    width: 100%;
    padding: 15px 0;
    border-bottom: 1px solid #f1f1f1;
    overflow: hidden;
}
.comment .list_block_l{
    width: 50px;
    height: 50px;
    float: left;
    position: relative;
    border-radius: 50%;
    overflow: hidden;
}
.comment .list_block_l img {
    width: 100%;
}
.comment .list_block_r {
    position: relative;
    margin-left: 65px;
}
.comment .list_block_r_info {
    color: #222;
    font-size: 18px;
}
.comment .list_block_r_text {
    color: #222;
    font-size: 14px;
    width: 75%
}
.comment .list_block_r_bottom {
    font-size: 14px;
    color: #757575;
    padding-top: 5px;
}
.comment .list_bottom_info_l {
    position: relative;
    float: left;
}
.comment .list_bottom_info_r {
    position: relative;
    text-align: right;
}
.comment .list_bottom_info_r span {
    display: inline-block;
    padding-right: 8px;
    padding-left: 3px;
}
.comment .list_title{
    position: relative;
    font-weight: bold;
    color: #222;
    font-size: 16px;
    width: 100%;
    border-bottom: 1px solid #f1f1f1;
    padding: 15px 0;
    margin-bottom: 5px;
}
 </style>
