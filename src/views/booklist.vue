<template>
    <div>
        <h2>豆瓣top250</h2>
        <div class="list">
            <router-link class="item" :to="{ name: 'detail', params: { id: book.id}}" v-for="book in books" :key="book.id">
                <div class="pic">
                    <img :src="book.image" :alt="book.origin_title">
                </div>
                <div class="title" >{{book.title}}</div>
                <div class="author">{{book.author}}</div>
            </router-link> 
            <div class="full-line"></div>
            <infinite-loading spinner="bubbles" :on-infinite="onInfinite" ref="infiniteLoading">
                <span slot="no-more">没有更多数据了哦</span>
            </infinite-loading>
        </div>
    </div>
</template>
<script>
import data from '@/data/test.json';
import InfiniteLoading from 'vue-infinite-loading';
export default {
    name: 'index',
    mounted() {
        // this.books = data.book;
    },
    data() {
        return {
            books: []
        }
    },
    components: {
        InfiniteLoading
    },
    methods: {
        onInfinite() {
           setTimeout(() => {
                const temp = [];
                for (let i = this.books.length + 1; i < this.books.length + 20; i++){
                    if (data.book.length === 0) {
                        this.$refs.infiniteLoading.$emit('$InfiniteLoading:complete');
                        break;
                    }
                    temp.push(data.book.shift());
                }
                this.books = this.books.concat(temp);
                this.$refs.infiniteLoading.$emit('$InfiniteLoading:loaded');
           }, 2000)
        }
    }
}
</script>
<style lang="scss" scoped>
@import '../assets/style/color';
    h2 {
        width: 100%;
        font-size: 3rem;
        text-align: center;
    }
    .list {
        /* Box-model */   
        display: flex; 
        flex-flow: row wrap;
        justify-content: center;
        width: 100%;
        margin: 0 auto 60px;
        /* Visual */
        background:  url(../assets/images/bg.png) 0 0 repeat-y;
        background-size: 100% auto; 
        .full-line {
            flex: 2 0 100%;
        }
        .item {
            /* Box-model */
            flex: 1 0 50%;
            display: block;
            // margin: 20px;
            /* Typography */
            text-align: center;
            &:hover {
                box-shadow: 0 0 10px rgba(0,0,0,.5);
            }
            .pic {
                width: 100%;
                img {
                    max-width: 100%;
                    height: 208px;
                }
            }
            .title {
                /* Typography */
                font-size: 1.6rem;
                font-weight: 700;
                line-height: 1.5em;
                /* Visual */
                color: #000;
            }
            .author {
                /* Typography */
                font-size: 1.4rem;
                /* Visual */
                color: #B0B0B0;
            }
        }
    }
</style>
