<template>
    <Layout>
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
            <el-button @click="share"
                       style="margin-left: 10px"
                       icon="el-icon-share"
                       type="warning"
                       plain
                       circle></el-button>
            <el-button type="primary"
                       icon="el-icon-edit"
                       round
                       plain
                       style="float: right;"
                       @click="goAdd">写博文</el-button>
        </el-card>

        <div v-if="$page.blogs.edges.length>0">
            <el-card shadow="hover"
                     v-for="(item,index) in $page.blogs.edges"
                     :key="'p'+index"
                     style="margin-bottom: 20px"
                     v-if="!item.node.hide">
                <div slot="header">
                    <el-row>
                        <el-col :span="16">
                            <span>
                                <a style="text-decoration:none;cursor:pointer"
                                   @click="goDetails(item.node.id)">
                                    <i class="el-icon-edit-outline"></i>&nbsp;&nbsp; {{item.node.title}}
                                </a>
                            </span>
                        </el-col>
                        <el-col :span="8">
                            <div style="text-align: right;">
                                <el-button @click="share('/user/blog/details/'+item.node.id)"
                                           style="padding: 3px 0"
                                           type="text"
                                           icon="el-icon-share"></el-button>
                                <el-button @click="editBlog(index)"
                                           style="padding: 3px 0"
                                           type="text"
                                           icon="el-icon-edit"
                                           v-if="token"></el-button>
                                <el-button @click="deleteBlog(index)"
                                           style="padding: 3px 0"
                                           type="text"
                                           icon="el-icon-delete"
                                           v-if="token"></el-button>
                            </div>
                        </el-col>
                    </el-row>
                </div>
                <div style="font-size: 0.9rem;line-height: 1.5;color: #606c71;">
                    最近更新 {{item.node.update}}
                </div>
                <div style="font-size: 1.1rem;line-height: 1.5;color: #303133;padding: 10px 0px 0px 0px">
                    {{item.node.description}}
                </div>
            </el-card>
            <div style="text-align: center">
                <pager :info="$page.blogs.pageInfo"></pager>
            </div>

        </div>

        <el-card shadow="never"
                 style="margin-bottom: 20px;padding: 20px 0px 20px 0px;text-align: center"
                 v-if="!$page.blogs || $page.blogs.length==0">
            <font style="font-size: 30px;color:#dddddd ">
                <b>还没有博客 (╯°Д°)╯︵ ┻━┻</b>
            </font>
        </el-card>
    </Layout>
</template>
<page-query>
query ($page: Int) {
    blogs: allStrapiBlog (perPage: 2, page: $page) @paginate  {
        pageInfo{
            totalPages
            currentPage
        }
        edges {
            node {
                id
                title
                content
                description
                publish
                update
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
            searchKey: "",
            token: ''
        }
    },
    components: {
        Pager
    },
    methods: {
        list () {

        },
        search () {
        },
        editBlog (index) {
        },
        deleteBlog (index) {

        },
        goAdd () {
        },
        goDetails (id) {
        },
        share (id) {
            console.log('分享', id)
        }
    }
}
</script>