<style>
    @import "../resources/css/reset.css";
    .container{
        font-family: "Microsoft YaHei";
        font-size: 16px;
        background-color: white;
        height: 100%;
    }
    .btn{
        float: right;margin-right: 20px;height:0.5rem;
    }
    .btn_line{
        height: 0.7rem;background-color:rgb(235,235,235);
        padding-top: 0.1rem;
    }
    .unique
        .mint-cell-wrapper{
            border: none;
            padding-top: 0.5rem;
        }
    .unique
        .mint-cell-title{
            width: 70%;
            flex: inherit;
        }
    .unique
        .mint-cell-value{
            width: 30%;
        }
    textarea{
        background-color: white;
    }

</style>
<template>
    <div class="container">
        <div class="unique" v-if="logShow1" v-for="(item,index) in list">
            <mt-cell style="clear: both;height: 80px;"
                     is-link
                     :to="'daikan_detail/'+item.id"
                     :title="item.gongsi" :label="formDate(item.dakatime)" >
                <span style="margin-top: -0.3rem;">
                    {{item.renyuan}}
                </span>
            </mt-cell>
            <div class="btn_line">
                <img src="../resources/images/daikan/chakan.png"  v-if="item.ispingjia" class="btn"  @click="check(item.id)" size="small">
                <img src="../resources/images/daikan/pingjia.png" v-if="!item.ispingjia" class="btn"  @click="pingjia(item.id)" size="small">
            </div>
        </div>
        <div v-if="logShow2" style="width: 100%;text-align: center;padding-top: 10%;">
            <img src="../resources/images/daikan/empty.png" width="130" alt="">
            <br>
            <span style="color: rgb(131,131,131)">您还没有任何带看记录，赶快记录一下吧！</span>
            <br>
            <span style="color: rgb(131,131,131)">我在<a href="javascript:;" style="color:rgb(28,119,212);" @click="daikan_add()">带看打卡</a>等你哟～</span>
        </div>
    </div>


</template>
<script>
export default{
    data(){
        return{
            list:[],
            logShow1:false,
            logShow2:false,
        }
    },
    methods:{
        pingjia(id){
            this.$router.push('/daikan_comment_add/'+id);
        },
        check(id){
            this.$router.push('/daikan_comment/'+id);
        },
        formDate(index){
            var newDate = new Date(index);
            return newDate.toLocaleString();
        },
        daikan_add(){
            this.$router.push('/daikan');
        }

    },
    mounted(){
        let para ={
            cookie:JSON.parse(localStorage.getItem('cookxs')),
        }
        this.$http.post(this.$api+'/yhcms/web/qddaka/getQdDakaForPerson.do',para).then((res)=>{
            var response = JSON.parse(res.data);
            if(response.success==true){
                this.list = response.data;
                if(this.list.length>0){
                    this.logShow1 = true;
                }else{
                    this.logShow2 = true;
                }
            }

        });
        document.title = '打卡记录';
    }
}
</script>