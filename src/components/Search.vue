<template>
    <section id = "search_main">
        <div id = "search">
            <input type = "text" v-focus v-model.trim = "search" v-on:input = "searchResult">
            <i class = "iconfont icon-sousuo1"></i>
            <router-link to = "/">
                <span>取消</span>
            </router-link>
        </div>
        <div class = "search_title">搜索结果</div>
        <template v-if = "results.length === 0 && search != ''">
            <div class = "not_find">没有相关影片</div>
        </template>
        <template v-else>
            <transition-group appear tag = "ul" name = "slide-in">
                <li v-for = "result in results" :key = "result.id">
                    <router-link :to = "'/video/' + result.id">
                        <img v-lazy = "baseUrl + result.img" alt = "">
                        <div class = "result_name">
                            <p>{{result.name}}</p>
                            <p>{{result.star}}分/{{result.time1}}</p>
                        </div>
                    </router-link>
                </li>
            </transition-group>
        </template>
    </section>
</template>

<script>
    import { mapState } from 'vuex';
    import { search, url } from '../data/fetchData.js';
    export default {
        name: 'search',
        data () {
            return {
                search: '',
                results: '',
                baseUrl: url + '/images/'
            }
        },
        computed: {

        },
        directives: {
            focus: {
                inserted: function (el) {
                    // 聚焦元素
                    el.focus();
                }
            },
            move: {
                inserted(el) {
                    document.body.appendChild(el);
                },
                unbind(el) {
                    document.body.removeChild(el);
                }
            }
        },
        created () {

        },
        methods: {
            searchResult(){
                if (this.search != '') {
                    search(this.search).then(res => {
                        // console.log(res);
                        this.results = res.data;
                    })
                } else {
                    this.results = '';
                }
            }
        }
    }

</script>

<style lang = "scss" scoped>
    #search_main {
        #search {
            padding: .2rem 0;
            background: #0fce0f;
            display: flex;
            justify-content: center;
            align-items: center;
            position: relative;
            input {
                width: 88%;
                height: .6rem;
                border: none;
                border-radius: .3rem;
                padding-left: .6rem;
                line-height: .6rem;
            }
            span {
                color: #fff;
                margin-left: .1rem;
                font-size: 14px;
            }
            i {
                position: absolute;
                top: 50%;
                transform: translateY(-50%);
                left: .25rem;
                font-size: 20px;
            }
        }
        .search_title {
            background: #f2f2f2;
            padding: .1rem 0;
            padding-left: .2rem;
            color: #333;
        }
        .not_find {
            text-align: center;
            font-size: 16px;
            margin-top: .1rem;
        }
        ul {
            li {
                padding: .12rem .2rem;
                &+li {
                    border-top: 1px solid #eee;
                }
                a {
                    font-size: 14px;
                    color: #333;
                    display: flex;
                    align-items: center;
                    img {
                        width: .7rem;
                        height: 1rem;
                    }
                    div {
                        margin-left: .2rem;
                        p {
                            margin: .05rem 0;
                        }
                    }
                }
            }
        }
    }
</style>