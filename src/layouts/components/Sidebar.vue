<template>
    <div class="slidebar">
        <el-card shadow="never">
            <el-menu :default-active="active"
                     @select="onSelect">
                <el-menu-item v-for="item in constantRouterMap"
                              v-if="item.meta&&item.meta.type=='user'&&(token||!item.meta.LoginRequired)&&(!mini||!item.meta.mini)"
                              :key="item.path"
                              :index="item.path">
                    <g-link class="g-link"
                            :to="item.path">
                        <i :class="item.meta.icon"></i>
                        <span slot="title">{{item.meta.title}}</span>
                    </g-link>
                </el-menu-item>
            </el-menu>
        </el-card>

        <el-card shadow="never"
                 style="margin-top: 20px;text-align: center">
            <div v-if="!token"
                 style="font-size: 0.9rem;line-height: 1.5;color: #606c71;">
                <el-tag type="danger"
                        size="small">&nbsp;</el-tag>&nbsp;&nbsp; Token未绑定&nbsp;&nbsp;
                <el-button type="text"
                           @click="openTokenDialog">绑定</el-button>
            </div>
            <div v-if="token"
                 style="font-size: 0.9rem;line-height: 1.5;color: #303133;">
                <el-tag type="success"
                        size="small">&nbsp;</el-tag>&nbsp;&nbsp; Token已绑定&nbsp;&nbsp;
                <el-button type="text"
                           @click="cancellation">注销</el-button>
            </div>
            <div style="margin-top: 10px;text-align: left">
                <el-alert title="Token获取"
                          type="info"
                          description="在 github-> settings-> developerSettings-> personalAccessTokens 勾选gist权限,获取Token. 详情参考README.md"
                          :closable="false">
                </el-alert>
            </div>
        </el-card>
    </div>
</template>

<script>
import { constantRouterMap } from './constantRouterMap'
export default {
    data () {
        return {
            constantRouterMap,
            active: "",
            parentUrl: "",
            menuList: []
        }
    },
    props: ['token', 'githubUsername', 'mini'],
    mounted () {
        let arr = this.$route.path.split("/")
        this.active = "/" + arr[1] + "/" + arr[2]
    },
    methods: {
        onSelect (index) {
            // this.$router.push(index)
            console.log(index)
        },
        openTokenDialog () {
            this.$refs.tokenDialog.open(() => {

            })
        },
        cancellation () {
            this.$store.dispatch("Cancellation")
        }
    }
}
</script>
<style lang="less">
.slidebar {
    .g-link {
        display: inline-block;
        width: 100%;
        color: inherit;
        &:hover {
            text-decoration: none;
        }
    }
}
</style>