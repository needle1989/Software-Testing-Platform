<template>

    <!--    页面布局-->
    <el-container class="home-container">
        <!--        头部区域-->
        <el-header>
            <div>
                <img src="../assets/img/tj_logo.png" height=40/>
            </div>
<!--            <div class="header-name">-->
<!--                {{username}}-->
<!--            </div>-->
        </el-header>
        <!--        页面主体-->
        <el-container>
            <!--            侧边栏-->
            <el-aside width="200px">
                <el-menu router
                         background-color="#545c64" text-color="#fff" active-text-color="#ffd04b">
				    <el-submenu index="1">
				        <template slot="title">QUESTIONS</template>
						<el-menu-item index="/Triangle">三角形类型</el-menu-item>
						<el-menu-item index="/SaleManagement">销售管理系统</el-menu-item>
						<el-menu-item index="/ComputerSale">电脑销售系统</el-menu-item>
						<el-menu-item index="/EBusinessPlat">电商平台系统</el-menu-item>
						<el-menu-item index="/Triangle">项目部署</el-menu-item>
						<el-menu-item index="/ElecCharge">电信收费问题</el-menu-item>
						<el-menu-item index="/CSSystem">C/S系统</el-menu-item>
						<el-menu-item index="/Calendar">万年历问题</el-menu-item>
						<el-menu-item index="/readerlist">测试</el-menu-item>
						<el-menu-item index="/AdminChangeUserInfo">测试</el-menu-item>
						<el-menu-item index="/AdminChangeUserPassword">测试</el-menu-item>
				    </el-submenu>
				    <el-submenu index="2">
				        <template slot="title">测试</template>
						<el-menu-item index="/BookReviewAdmin">浏览书籍推荐</el-menu-item>
						<el-menu-item index="/AllBook">获取全部书籍</el-menu-item>
						<el-menu-item index="/SearchBox">搜索书籍</el-menu-item>
						<el-menu-item index="/BookRegister">新建书籍</el-menu-item>
						<el-menu-item index="/BookReturns">归还书籍</el-menu-item>
						<el-menu-item index="/search">搜索借阅记录</el-menu-item>
				    </el-submenu>
					<el-submenu index="3">
					    <template slot="title">自习室管理</template>
					    <el-menu-item index="/RoomList">获取自习室信息</el-menu-item>
					</el-submenu>
					<el-submenu index="4">
					    <template slot="title">管理员内务</template>
						<el-menu-item index="/adminlist">获取管理员列表</el-menu-item>
						<el-menu-item index="/AdminChangeIf">修改管理员信息</el-menu-item>
						<el-menu-item index="/AdminChangePass">修改管理员密码</el-menu-item>
						<el-menu-item index="/AdminRegister">管理员注册</el-menu-item>
						<el-menu-item index="/AdminInfo">使用者信息</el-menu-item>
					</el-submenu>
                </el-menu>
            </el-aside>
            <!--            右侧内容主体-->
            <el-main>
                <router-view></router-view>
            </el-main>
        </el-container>
    </el-container>
</template>

<script>
    export default {
        data(){
            return{
                username: window.sessionStorage.getItem('token'),
                booklist:
                {
                    book_id: '',
                }
            }
        },
        mounted: function () {
            this.bookcategory();
           
        },
        methods: {
            bookcategory() {
                let status = 200;
                let Token = window.sessionStorage.getItem('token');
                this.$http.get('/apip/api/books/', {
                    hearders: {
                        'Authorization': Token,
                    },
                })
                    .then(function (response) {
                        var data = JSON.stringify(response.data);
                       // alert(data);
                        window.sessionStorage.setItem('book', data);
                    }).catch(function (response) {
                        console.log(response);
                    });
            },
        }
    };
</script>

<style scoped>
    .home-container {
        height: 100%;
    }
	.el-container{
		background-color: rgb(84, 92, 100);
	}
	.el-menu{
		border-right:0;
	}

    .el-header {
		background: url(../assets/img/head_bg.jpg) top center;
        background-color: rgb(255, 255, 255);
        display: flex;
        justify-content: space-between;
        margin-left: 0;
        padding-left: 0;

    }

    .el-aside {
        /*background-color: gray;*/
    }

    .el-main {
        /*background-color: lightgoldenrodyellow;*/
		background-color: white;
    }

    .out-button{
        display: flex;
        justify-content: center;
        align-items: center;
    }


    .header-name{
        display: flex;
        justify-content: center;
        align-items: center;

    }

</style>
