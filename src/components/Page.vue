<template>
    <div class="homeBox">
        <el-container>
            <el-header>
                <div>
                    <el-menu
                        :default-active="activeIndex2"
                        class="el-menu-demo"
                        mode="horizontal"
                        @select="handleSelect"
                        background-color="#545c64"
                        text-color="#fff"
                        active-text-color="#ffd04b">
                        <el-menu-item index="1">处理中心</el-menu-item>
                        <el-submenu index="2">
                            <template slot="title">我的工作台</template>
                            <el-menu-item index="2-1">选项1</el-menu-item>
                            <el-menu-item index="2-2">选项2</el-menu-item>
                            <el-menu-item index="2-3">选项3</el-menu-item>
                        </el-submenu>
                        <el-menu-item index="3"><a href="https://www.ele.me" target="_blank" style="text-decoration: none;">订单管理</a></el-menu-item>
                        <el-menu-item index="4" style="float: right;">注册</el-menu-item>
                        <el-menu-item index="5" style="float: right;">登录</el-menu-item>
                    </el-menu>
                </div>
            </el-header>
            <el-container style="height:100%;">
                <el-aside width="200px">
                    <div>
                        <el-menu default-active="1-4-1" class="el-menu-vertical-demo" @open="handleOpen" @close="handleClose" :collapse="isCollapse" background-color="#434c58" text-color="#fff" active-text-color="#ffd04b">
                            <el-submenu index="1">
                                <template slot="title">
                                    <i class="el-icon-location"></i>
                                    <span slot="title">导航一</span>
                                </template>
                                <el-menu-item-group>
                                    <span slot="title">分组一</span>
                                    <el-menu-item index="1-1">选项1</el-menu-item>
                                    <el-menu-item index="1-2">选项2</el-menu-item>
                                </el-menu-item-group>
                                <el-menu-item-group title="分组2">
                                    <el-menu-item index="1-3">选项3</el-menu-item>
                                </el-menu-item-group>
                                <el-submenu index="1-4">
                                    <span slot="title">选项4</span>
                                    <el-menu-item index="1-4-1">选项1</el-menu-item>
                                </el-submenu>
                            </el-submenu>
                        <el-menu-item index="2">
                            <i class="el-icon-menu"></i>
                            <span slot="title">导航二</span>
                        </el-menu-item>
                        <el-menu-item index="3">
                            <i class="el-icon-setting"></i>
                            <span slot="title">导航三</span>
                        </el-menu-item>
                        </el-menu>
                    </div>
                </el-aside>

                <el-container>
                    <el-main>

                        <div style="padding-bottom: 20px;padding-left: 30%">
                            <el-row>
                                <el-col :span="10" class="center">
                                    <el-input
                                        v-model="search"
                                        @focus="focus"
                                        @blur="blur"
                                        @keyup.enter.native="searchHandler"
                                        placeholder="这搜索就礼璞"
                                        >
                                    <el-button slot="append" icon="el-icon-search" id="search" @click="searchHandler"></el-button>
                                    </el-input>
                                    <!---设置z-index优先级,防止被走马灯效果遮挡-->
                                    <el-card
                                        @mouseenter="enterSearchBoxHanlder"
                                        v-if="isSearch"
                                        class="box-card"
                                        id="search-box"
                                        style="position:absolute;z-index:15;width:40%"
                                        >
                                        <dl v-if="isHistorySearch">
                                            <dt class="search-title" v-show="history">历史搜索</dt>
                                            <dt class="remove-history" v-show="history" @click="removeAllHistory">
                                                <i class="el-icon-delete"></i>清空历史记录
                                            </dt>
                                            <el-tag
                                                v-for="search in historySearchList"
                                                :key="search.id"
                                                closable
                                                :type="search.type"
                                                @close="closeHandler(search)"
                                                style="margin-right:5px; margin-bottom:5px;"
                                                >{{search.name}}</el-tag>
                                            <dt class="search-title">热门搜索</dt>
                                            <dd v-for="search in hotSearchList" :key="search.id">{{search}}</dd>
                                        </dl>
                                        <dl v-if="isSearchList">
                                            <dd v-for="search in searchList" :key="search.id">{{search}}</dd>
                                        </dl>
                                    </el-card>
                                </el-col>
                            </el-row>
                        </div>

                        <div>
                            <template>
                                <el-carousel :interval="4000" type="card" height="200px">
                                    <el-carousel-item v-for="item in 6" :key="item">
                                    <h3>{{ item }}</h3>
                                    </el-carousel-item>
                                </el-carousel>
                            </template>
                        </div>

                        <el-row>
                            <el-col :span="4" v-for="(o, i) in List" :key="o.name" :offset="2" style="margin-bottom:40px">
                                <el-card :body-style="{ padding: '0px' }">
                                    <img :src=o.url class="image">
                                    <div style="padding: 14px;">
                                        <span>{{ o.name }}</span>
                                        <div class="bottom clearfix">
                                            <time class="time">{{ o.time }}</time>
                                            <el-button type="text" class="button" @click="cardClick(o.path)">让我康康</el-button>
                                        </div>
                                    </div>
                                </el-card>
                                <div v-if="i%4==0">
                                    
                                </div>
                            </el-col>
                        </el-row>

                    </el-main>
                    <el-footer>
                        <div>

                        </div>
                    </el-footer>
                </el-container>

            </el-container>
        </el-container>
    </div>
</template>


<script>
    import RandomUtil from "@/utils/randomUtil";
    import Store from "@/utils/store";

  export default {
    name:'Page',
    data() {
      return {
        activeIndex: '1',
        activeIndex2: '1',
        input: '',
        currentDate: new Date(),
        isCollapse: false,
        List:[
            {name:'紫罗兰永恒花园', time: ' 2020-01-10(中国大陆) / 2019-09-06(日本)', url:require('@/assets/anim.jpg'), path: '/about'},
            {name:'紫罗兰永恒花', time: ' 2020-01-10(中国大陆) / 2019-09-06(日本)', url:require('@/assets/anim.jpg'), path: '/about'},
            {name:'紫罗兰永恒园', time: ' 2020-01-10(中国大陆) / 2019-09-06(日本)', url:require('@/assets/anim.jpg'), path: '/about'},
            {name:'紫罗兰永花园', time: ' 2020-01-10(中国大陆) / 2019-09-06(日本)', url:require('@/assets/anim.jpg'), path: '/about'},
            {name:'紫罗兰恒花园', time: ' 2020-01-10(中国大陆) / 2019-09-06(日本)', url:require('@/assets/anim.jpg'), path: '/about'},
        ],

        search: "", //当前输入框的值
        isFocus: false, //是否聚焦
        hotSearchList: ["暂无热门搜索"], //热门搜索数据
        historySearchList: [], //历史搜索数据
        searchList: ["暂无数据"], //搜索返回数据,
        history: false,
        types: ["", "success", "info", "warning", "danger"] //搜索历史tag式样
     };
    },
    methods: {
      handleSelect(key, keyPath) {
        console.log(key, keyPath);
      },
      handleOpen(key, keyPath) {
        console.log(key, keyPath);
      },
      handleClose(key, keyPath) {
        console.log(key, keyPath);
      },
      cardClick(Keypath) {
          this.$router.push({path: Keypath})
      },

      focus() {
        this.isFocus = true;
        this.historySearchList =
            Store.loadHistory() == null ? [] : Store.loadHistory();
        this.history = this.historySearchList.length == 0 ? false : true;
        },
        blur() {
            var self = this;
            this.searchBoxTimeout = setTimeout(function() {
                self.isFocus = false;
            }, 300);
        },
        enterSearchBoxHanlder() {
            clearTimeout(this.searchBoxTimeout);
        },
        searchHandler() {
            //随机生成搜索历史tag式样
            let n = RandomUtil.getRandomNumber(0, 5);
            let exist =
                this.historySearchList.filter(value => {
                return value.name == this.search;
                }).length == 0
                ? false
                : true;
            if (!exist) {
                this.historySearchList.push({ name: this.search, type: this.types[n] });
                Store.saveHistory(this.historySearchList);
            }
            this.history = this.historySearchList.length == 0 ? false : true;
        },
        closeHandler(search) {
            this.historySearchList.splice(this.historySearchList.indexOf(search), 1);
            Store.saveHistory(this.historySearchList);
            clearTimeout(this.searchBoxTimeout);
            if (this.historySearchList.length == 0) {
                this.history = false;
            }
        },
        removeAllHistory() {
            Store.removeAllHistory();
        }
      
    },
    props:{
		msg: String
    },
    computed: {
        isHistorySearch() {
        return this.isFocus && !this.search;
        },
        isSearchList() {
        return this.isFocus && this.search;
        },
        isSearch() {
        return this.isFocus;
        }
    }
  }
</script>
<style>
    html,
    body,
    .homeBox,
    .el-main,
    .el-header,
    .el-container{
        margin:0;
        padding: 0;
        height: 100%;
        width: 100%;
    }

    .el-aside{
        background-color: #475669;
    }

    .el-main .el-row{
        padding-right: 30px;
    }

    .el-carousel__item h3 {
        color: #475669;
        font-size: 14px;
        opacity: 0.75;
        line-height: 200px;
        margin: 0;
    }
    
    .el-carousel__item:nth-child(2n) {
        background-color: #99a9bf;
    }
    
    .el-carousel__item:nth-child(2n+1) {
        background-color: #d3dce6;
    }

    .time {
        font-size: 13px;
        color: #999;
    }
    
    .bottom {
        margin-top: 13px;
        line-height: 12px;
    }

    .button {
        padding: 0;
        float: right;
    }

    .image {
        width: 100%;
        display: block;
    }

    .clearfix:before,
    .clearfix:after {
        display: table;
        content: "";
    }
    
    .clearfix:after {
        clear: both
    }

    .el-menu-vertical-demo:not(.el-menu--collapse) {
        width: 200px;
        min-height: 400px;
    }

    .left {
        margin-left: 200px;
    }
    .center {
        margin-top: 15px;
        margin-left: 200px;
    }
    #search {
        background-color: cornflowerblue;
        border-radius: 0%;
    }
    .search-title {
        color: #bdbaba;
        font-size: 15px;
        margin-bottom: 5px;
    }
    .remove-history {
        color: #bdbaba;
        font-size: 15px;
        float: right;
        margin-top: -22px;
    }
    #search-box {
        width: 555px;
        height: 300px;
        margin-top: 0px;
        padding-bottom: 20px;
    }
</style>