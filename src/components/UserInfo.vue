<template>
  <div class="UserInfo">
    <div class="loading" v-if="isLoading">
      <img src="../assets/loading.gif" alt="">
    </div>
    <div class="userInformation" v-else>
      <section>
        <img :src="userinfo.avatar_url" alt="">
        <div class="loginName">{{ userinfo.loginname }}</div>
        <p>
          {{ userinfo.score }}积分
        </p>
        <p>
          注册时间：{{ userinfo.create_at | formatDate }}
        </p>
      </section>
      <div class="replies">
        <p>回复的主题</p>
        <ul>
          <li v-for="item in userinfo.recent_replies">
            <router-link :to="{
              name:'post_content',
              params: {
                id: item.id
              }
            }">
              {{ item.title }}
            </router-link>
          </li>
        </ul>
      </div>
      <div class="topics">
        <p>创建的主题</p>
        <ul>
          <li v-for="item in userinfo.recent_topics">
            <router-link :to="{
              name: 'post_content',
              params: {
                id: item.id
              }
            }">
              {{ item.title }}
            </router-link>
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "UserInfo",
  data() {
    return {
      isLoading: false,
      userinfo: {}
    }
  },
  methods: {
    getData() {
      this.$http.get(`https://cnodejs.org/api/v1/user/${this.$route.params.name}`)
        .then(res => {
          this.isLoading = false;
          this.userinfo = res.data.data;
        })
        .catch(function (err) {
          console.log(err)
        })
    }
  },
  beforeMount() {
    this.isLoading = true;
    this.getData();
  }
}
</script>

<style scoped>
.userInformation {
  background-color: white;
  width: 75%;
  margin: 10px auto;
}

.userInformation section {
  padding: 12px;
}

.userInformation img {
  width: 30px;
}

section {
  position: relative;
}

.userInformation .loginName {
  position: absolute;
  left: 50px;
  top: 20px;
  font-weight: bold;
}

.userInformation li {
  list-style: none;
}

.userInformation .replies,
.userInformation .topics {
  font-size: 0.72em;
  border-top: 10px #DDD solid;
  padding: 4px 0 0 6px;

}

.userInformation  li {
  padding: 4px 0 4px 12px;
  white-space: nowrap;
  font-size: 0.72rem;
  text-overflow: ellipsis;
  overflow: hidden;
  line-height: 30px;
  vertical-align: middle;
}

.userInformation  a {
  color: #094E99;
  text-decoration: none;
}
</style>
