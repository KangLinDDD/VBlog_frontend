<template>
    <Layout class="social">
        <el-card shadow="never"
                 style="margin-bottom: 20px">
            <el-input placeholder="请输入关键字"
                      v-model="searchKey"
                      clearable
                      style="width: 300px"></el-input>
            <el-button @click="search"
                       icon="el-icon-search"
                       style="margin-left: 10px"
                       circle
                       plain></el-button>
            <el-button @click="$share()"
                       icon="el-icon-share"
                       type="warning"
                       style="margin-left: 10px"
                       plain
                       circle></el-button>
        </el-card>

        <div v-if="projects&&projects.length>0">
            <el-card shadow="hover"
                     v-for="(item,index) in projects"
                     :key="'pro'+index"
                     style="margin-bottom: 20px"
                     v-if="!item.node.hide">
                <div slot="header">
                    <el-row>
                        <el-col :span="16">
                            <span>
                                <a style="text-decoration:none;cursor:pointer"
                                   @click="goDetails(item.node.title)">
                                    <i class="el-icon-service"></i>&nbsp;&nbsp; {{item.node.title}}
                                </a>
                            </span>
                        </el-col>
                        <el-col :span="8">
                            <div style="text-align: right;">
                                <el-button @click="goGithub(item.node.git_url)"
                                           style="padding: 3px 0"
                                           type="text"
                                           icon="el-icon-back">前往GitHub</el-button>
                                <el-button @click="$share('/user/project/details/'+item.node.title)"
                                           style="padding: 3px 0"
                                           type="text"
                                           icon="el-icon-share"></el-button>
                            </div>
                        </el-col>
                    </el-row>
                </div>
                <div style="font-size: 0.9rem;line-height: 1.5;color: #606c71;">
                    最近更新 {{item.node.updateTime}}
                </div>
                <div style="font-size: 1.1rem;line-height: 1.5;color: #303133;padding: 10px 0px 0px 0px">
                    {{item.node.description}}
                </div>
                <div style="font-size: 1.1rem;color: #303133;padding: 10px 0px 0px 0px">
                    <el-row>
                        <el-col :span="16"
                                style="padding-top: 5px">
                            <el-tooltip effect="dark"
                                        :content="'star '+item.node.star"
                                        placement="bottom">
                                <i class="el-icon-star-off"
                                   style="margin: 0px 5px 0px 0px"></i>
                            </el-tooltip>
                            {{item.node.star}}
                            <el-tooltip effect="dark"
                                        :content="'watch '+item.node.watch"
                                        placement="bottom">
                                <i class="el-icon-view"
                                   style="margin: 0px 5px 0px 15px"></i>
                            </el-tooltip>
                            {{item.node.watch}}
                            <el-tooltip effect="dark"
                                        :content="'fork '+item.node.fork"
                                        placement="bottom">
                                <i class="el-icon-bell"
                                   style="margin: 0px 5px 0px 15px"></i>
                            </el-tooltip>
                            {{item.node.fork}}
                        </el-col>
                        <el-col :span="8"
                                class="tags"
                                style="text-align: right;">
                            <el-tag size="small"
                                    type="danger"
                                    v-if="item.node.license">{{item.node.license.license}}</el-tag>
                            <el-tag size="small"
                                    type="success">{{item.node.tag.tag}}</el-tag>
                        </el-col>
                    </el-row>
                </div>
            </el-card>
            <div style="text-align: center">
                <pager :info="$page.list.pageInfo"></pager>
            </div>
        </div>

        <el-card shadow="never"
                 style="margin-bottom: 20px;padding: 20px 0px 20px 0px;text-align: center"
                 v-if="!projects||projects.length==0">
            <font style="font-size: 30px;color:#dddddd ">
                <b>还没有开源项目 (╯°Д°)╯︵ ┻━┻</b>
            </font>
        </el-card>
    </Layout>
</template>
<page-query>
query ($page: Int) {
    list: allStrapiProject (perPage: 2, page: $page) @paginate {
        pageInfo{
            totalPages
            currentPage
        }
        edges {
            node {
                id
                title
                update
                license {
                    license
                }
                tag {
                    tag
                }
                fork
                star
                watch
                git_url
            }
        }
    }
}
</page-query>
<script>
import { Pager } from 'gridsome'
export default {
    data () {
        return {
            query: {
                page: 1,
                pageSize: 5,
                pageNumber: 1
            },
            searchKey: "",
            token: ''
        }
    },
    components: {
        Pager
    },
    computed: {
        projects () {
            this.$page.list.edges.map(edge => {
                edge.node.hide = false
                console.log(edge.node)
            })
            return this.$page.list.edges
        }
    },
    methods: {
        list () {
        },
        search () {
        },
        goDetails (title) {
            // this.$router.push("/user/project/details/" + title)
        },
        goGithub (url) {
            window.open(url)
        }
    }
}
</script>
<style lang="less">
.social {
    .tags {
        .el-tag {
            margin-left: 4px;
        }
    }
}
</style>