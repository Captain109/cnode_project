<template>
  <div class="UserInfo">
    <div class="loading" v-if="isLoading">
      <img src="../assets/loading.gif" alt="">
    </div>
    <div class="userInformation" v-else>
      <section>
        <img :src="userinfo.avatar_url" alt="">
        <span>{{ userinfo.loginname }}</span>
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
            <route-link :to="{
              name:'post_content',
              params: {
                id: item.id
              }
            }">
              {{ item.title }}
            </route-link>
          </li>
        </ul>
      </div>
      <div class="topics">
        <p>创建的主题</p>
        <ul>
          <li v-for="item in userinfo.recent_topics">
            <route-link :to="{
              name: 'post_content',
              params: {
                id: item.id
              }
            }">
              {{ item.title }}
            </route-link>
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
      this.$http.get(`https://condejs.org/api/v1/user/${this.$route.params.name}`)
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

</style>
