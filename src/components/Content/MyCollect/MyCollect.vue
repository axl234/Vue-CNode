<template>
  <div class="collect-wrap">
    <back-bar :title="'我的收藏'"></back-bar>
    <ul class="collect">
      <li v-for="item in myCollections">
        <article-card :article="item"></article-card>
      </li>
    </ul>
  </div>
</template>

<script type="text/ecmascript-6">
  import BackBar from '../../BackBar/BackBar'
  import ArticleCard from '../ArticleCard/ArticleCard'
  import { mapGetters } from 'vuex'

  export default {
    components: {
      BackBar,
      ArticleCard
    },
    computed: {
      ...mapGetters([
        'myCollections'
      ])
    },
    activated () {
      window.scrollTo(0, 0)
      this.$store.dispatch('changeLoadingStatus', true)
      this.axios.get(`https://cnodejs.org/api/v1/topic_collect/${this.$route.params.loginname}`)
        .then(res => {
          this.$store.dispatch('changeLoadingStatus', false)
          this.$store.dispatch('init_my_collections', res.data.data)
        })
    },
    deactivated () {
      this.isLoading = true
    }
  }
</script>

<style lang="scss" rel="stylesheet/scss">
  .collect-wrap {
    width: 100%;
    min-height: 100%;
    padding-top: 48px;
  }
</style>
