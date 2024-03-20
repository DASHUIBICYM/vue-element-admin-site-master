<template>
    <div>
        <div>文章列表</div>
        <div>
            <a
                v-for="(item, index) in pageList"
                :key="index"
                class="article-item"
                :href="item.path"
            >
                <!-- <div class="msg-title">{{ item.title }}</div> -->
                <img
                    :src="
                        $withBase(
                            item.frontmatter.backgroundImg
                                ? item.frontmatter.backgroundImg
                                : '2024/1631457913111.jpg'
                        )
                    "
                    alt=""
                />
                <p class="title">{{ item.title }}</p>
                <p v-if="item.frontmatter.backgroundImg" class="title">
                    {{ item.frontmatter.backgroundImg }}
                </p>
                <p class="time">{{ item.frontmatter.description }}</p>
            </a>
        </div>
        <pagination
            :page-no="page.index"
            :page-size="page.size"
            :total="page.total"
            :continues="3"
            @change-page-no="getPageNo"
            @change-page-size="getPageSize"
        />
    </div>
</template>

<script>
import pagination from "./pagination.vue"
export default {
    components: {
        pagination,
    },
    name: "articleList",
    data() {
        return {
            articleList: [],
            pageList: [],
            page: {
                index: 1,
                size: 10,
                total: 0,
            },
        }
    },
    created() {
        this.init()
        this.getImg()
    },
    methods: {
        init() {
            this.articleList = this.getListArr()
            this.getList()
            this.page.total = this.articleList.length
            console.log(this.page.total, this.articleList)
        },
        getListArr() {
            const arr = this.$site.pages.filter((item) => {
                return item.path.includes("/messages/list/")
            })
            return arr
        },
        getPageNo(i) {
            this.page.index = i
            this.getList()
        },

        getPageSize(size) {
            this.page.size = size
            this.page.index = 1
            this.getList()
        },

        getList() {
            const { index, size } = this.page
            this.pageList = this.articleList.slice((index - 1) * size, index * size)
        },
        getImg() {
            const src = this.$withBase("fsefwefw")
            console.log(src)
        },
    },
}
</script>
<style>
.article-list {
    .article-item {
        display: flex;
        justify-content: space-between;
        align-items: center;
        &:not(:last-child) {
            border-bottom: 1px dashed var(--c-border);
        }
        .title {
            flex: 1;
            white-space: nowrap;
            overflow: hidden;
            text-overflow: ellipsis;
            color: var(--c-text);
            padding-left: 16px;
            position: relative;
            &:before {
                content: "";
                position: absolute;
                top: 50%;
                left: 2px;
                width: 6px;
                height: 6px;
                transform: rotate(45deg);
                background: var(--c-brand);
                margin-top: -3px;
            }
        }
        .time {
            margin-left: 20px;
            font-size: 14px;
            font-weight: normal;
            color: var(--c-text-lightest);
        }
    }
}
.msg-title {
    font-size: 18px;
    font-weight: 500;
    color: #3a3a3a;
}
</style>
