<template>
    <Layout>
        <el-card shadow="never"
                 style="min-height: 400px;margin-bottom: 20px;padding: 0px 0px 20px 0px">
            <el-tabs v-model="activeTab"
                     type="card"
                     @tab-click="onSelect">
                <el-tab-pane :label="'粉丝 '+ $page.fans.edges.length"
                             name="followers"
                             style="padding: 5px">
                    <div>
                        <div v-if="$page.fans.edges.length">
                            <el-row style="min-height: 200px; ">
                                <el-col :span="8"
                                        v-for="(item,index) in $page.fans.edges"
                                        :key="item.node.id"
                                        style="padding: 10px">
                                    <el-card shadow="hover"
                                             style="font-size: 13px;color: #606266;line-height: 20px">
                                        <i class="el-icon-star-off"></i>&emsp;
                                        <a @click="$router.push(`/social/details/${item.node.name}`)"
                                           style=" text-decoration:none;cursor:pointer">{{item.node.name}}</a>
                                        <br>
                                        <i class="el-icon-message"></i>&emsp;
                                        <a :href="item.node.git_url"
                                           target="_blank"
                                           style=" text-decoration:none;cursor:pointer">TA的主页</a>
                                        <br>
                                        <img :src="GRIDSOME_API_URL + item.node.image.image.url"
                                             style="width: 100%;border-radius:5px;margin-top: 5px">
                                    </el-card>
                                </el-col>
                            </el-row>
                            <pager :info="$page.fans.pageInfo"></pager>
                        </div>
                        <div style="min-height: 300px;margin-bottom: 20px;padding: 20px 0px 20px 0px;text-align: center"
                             v-else>
                            <font style="font-size: 30px;color:#dddddd ">
                                <b>(￢_￢) 没有一个粉丝</b>
                            </font>
                        </div>
                    </div>
                </el-tab-pane>
                <el-tab-pane :label="'关注 ' + $page.allows.edges.length"
                             name="following"
                             style="padding: 5px">
                    <div>
                        <div v-show="$page.allows.edges.length">
                            <el-row style="min-height: 200px; ">
                                <el-col :span="8"
                                        v-for="(item,index) in $page.allows.edges"
                                        :key="'following'+index"
                                        style="padding: 10px">
                                    <el-card shadow="hover"
                                             style="font-size: 13px;color: #606266;line-height: 20px">
                                        <i class="el-icon-star-off"></i>&emsp;
                                        <a @click="$router.push(`/user/social/details/${item.node.name}`)"
                                           style=" text-decoration:none;cursor:pointer">{{item.node.name}}</a>
                                        <br>
                                        <i class="el-icon-message"></i>&emsp;
                                        <a :href="item.node.git_url"
                                           target="_blank"
                                           style=" text-decoration:none;cursor:pointer">TA的主页</a>
                                        <br>
                                        <img :src="GRIDSOME_API_URL + item.node.image.image.url"
                                             style="width: 100%;border-radius:5px;margin-top: 5px">
                                    </el-card>
                                </el-col>
                            </el-row>
                            <pager :info="$page.allows.pageInfo"></pager>
                        </div>
                        <div style="min-height: 300px;margin-bottom: 20px;padding: 20px 0px 20px 0px;text-align: center"
                             v-show="$page.allows.edges.length === 0">
                            <font style="font-size: 30px;color:#dddddd ">
                                <b>(￢_￢) 还没有关注一个人</b>
                            </font>
                        </div>
                    </div>
                </el-tab-pane>
            </el-tabs>
        </el-card>
    </Layout>
</template>
<page-query>
query ($page: Int) {
    fans: allStrapiFan  (perPage: 2, page: $page) @paginate {
        pageInfo{
            totalPages
            currentPage
        }
        edges {
            node {
                id
                image {
                    image {
                        url
                    }
                }
                git_url
                name
            }
        }
    }
    allows: allStrapiAllow (perPage: 2, page: $page) @paginate {
        pageInfo{
            totalPages
            currentPage
        }
        edges {
            node {
                id
                image {
                    image {
                        url
                    }
                }
                git_url
                name
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
            activeTab: "followers",
            followers: {
                query: {
                    page: 1,
                    pageSize: 9,
                    pageNumber: 1
                },
                loading: false,
                list: []
            },
            following: {
                query: {
                    page: 1,
                    pageSize: 9,
                    pageNumber: 1
                },
                loading: false,
                list: []
            },
            githubUsername: '',
            followersTotal: '',
            followingTotal: ''
        }
    },
    components: {
        Pager
    },
    methods: {
        onSelect (tab) {
            console.log(this.activeTab)
        },
        listFollowers () {

        },
        listFollowing () {

        }
    }
}
</script>