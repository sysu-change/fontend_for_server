<template>
  <div>
    <h3 class="logout-title">是否注销</h3>

    <el-button class="yes-button" type="primary" @click="Signout()">是</el-button>
    <el-button class="no-button" type="primary" @click="back()">否</el-button>
  </div>
</template>
<script>
export default {
  methods: {
    Signout: function() {
      this.SignoutforUser(this);
    },
    SignoutforUser: function(vm) {
      var axios = {
        method: "delete",
        url: "/module/logout",
        widthCredentials: false
      };
      vm.$http(axios)
        .then(function(res) {
          if (res.status == 200) {
            if (res.data.code == 200) {
              vm.$router.push({ path: "/Hello" });
              sessionStorage.clear();
            } else {
              vm.$message({
                showClose: true,
                message: res.data.msg,
                type: "error"
              });
            }
          } else {
            vm.$message({
              showClose: true,
              message: "request failed",
              type: "error"
            });
          }
        })
        .catch(function(err) {
          console.log(err);
        });
      //this.$router.push({path: '/Hello'});
    },

    back: function() {
      this.$router.push({ path: "/User" });
    }
  }
};
</script>

<style scoped>
.logout-title {
  margin-top: 5%;
  text-align: center;
  font: 2.3em sans-serif;
  color: #00b38a;
  margin-bottom: 5%;
}
.yes-button {
  position: relative;
  background-color: #ffffff;
  border: 1.5px solid #00b38a;
  color: #00b38a;
  left: 0%;
  border-radius: 4px;
  width: 150px;
  height: 40px;
  margin: 2%;
}
.no-button {
  position: relative;
  background-color: #ffffff;
  border: 1.5px solid #00b38a;
  color: #00b38a;
  left: 0%;
  border-radius: 4px;
  width: 150px;
  height: 40px;
  margin: 2%;
}
.cc {
  background-color: azure;
}
</style>




