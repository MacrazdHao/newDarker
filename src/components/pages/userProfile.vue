<template>
  <!--<div class="user-container">-->
    <!--<div class="user-info">-->
      <!--<div class="user-avatar-wrap">-->
        <!--<img src="" class="user-avatar-img">-->
      <!--</div>-->
      <!--<div class="user-detail">-->
        <!--<div class="user-nick"></div>-->
      <!--</div>-->
    <!--</div>-->
  <!--</div>-->
  <div class="user-p-container">
    <div class="profile-outter">
      <div class="profile-wrapper">
        <div class="u-avatar">
          <img class="avatar-img" :src="userInfo.face?userInfo.face:'../../../static/img/noAvatar.jpg'"/>
        </div>
        <div class="u-infos">
          <div class="u-nick">
            <span style="font-size: 22px">{{userInfo.nick}}</span>
          </div>
          <div class="u-id">
            <span >id:{{userInfo.uid?userInfo.uid:userInfo.userId}}</span>
          </div>
          <div class="u-sex">
            <span >性别 {{userInfo.sex}}</span>
          </div>
        </div>
      </div>
      <div class="u-sign">
        <p class="u-sign-title">个人签名</p>
        <p>{{userInfo.sign}}</p>
      </div>
    </div>

    <div class="comment-container1">
      <real-comment
        :oid="uid"
        :type="4"
        :rpid="rpid"
      >
      </real-comment>
    </div>
  </div>
</template>

<script>
  import api from "../../api";
  import realComment from "../comment/RealComment.vue";
  export default {
    name: "userProfile",
    data() {
      return {
        userInfo: "",
        uid: "",
        rpid: ""
      }
    },
    components:{
      "real-comment": realComment
    },
    methods: {
      async initUserInfo(uid) {
        let res = await api.getUserInfoByUid(uid);
        let rd = res.data;
        if (rd.code === 0) {
          console.log("获取用户信息成功,", rd.data);
          this.userInfo = rd.data;
        }
        else {
          this.$message({
            message: rd.msg,
            type: "error"
          });
        }
      }
    },
    created() {
      console.log("UserProfile.vue created!");
      let uid = this.$route.params.uid;
      let rpid = this.$route.query.rpid;
      console.log("uid is:", uid);
      if (uid) {
        this.uid = parseInt(uid);
        this.initUserInfo(uid);
      }
      else {
        console.log("uid为空");
      }
      if(rpid){
        this.rpid = rpid;
      }
    }
  }
</script>

<style scoped>
  .user-p-container{
    margin: 0 auto;
    margin-top: 40px;
  }
  .profile-wrapper {
    display: inline-flex;
    animation: ShowVideo 0.4s;
    margin: 50px 10px 20px 10px;
  }
  .u-avatar {
    position: relative;
    float: left;
    width: 100px;
    height: 100px;
    border: 2px solid hsla(0, 0%, 100%, .4);
    border-radius: 52px;
  }
  .avatar-img {
    width: 100px;
    height: 100px;
    /* background: #fff; */
    border-radius: 48px;
  }
  .u-infos {
    float: left;
    color: #fff;
    /* font-size: 0; */
    margin: 10px 0 0 20px;
  }
  .u-infos span {
    display: inline-block;
    margin: 3px;
    font-weight: 600;
    line-height: 20px;
    font-size: 16px;
    vertical-align: middle;
  }
  .comment-container1{
    margin: 68px auto;
    margin-bottom: 255px;
  }
  .u-sign {
    color: #cccccc;
  }
  .u-sign-title {
    margin: 10px
  }
</style>
