<template>
  <div id="app" :class='{"is-dashboard": isDashboard}'>
    <!-- 头部 -->
    <header-nav></header-nav>
    
    <div class='content-sidebar-wrapper'>
      <!-- 侧边栏 -->
      <side-bar class='sidebar-wrapper'></side-bar>
      
      <!-- 内容区 -->
      <main class='content-wrapper' ref='scrollWrapper'>
        <router-view class='content'></router-view>
      </main>
    </div>
    
    <!-- 回顶部 -->
    <rotating-dashed-border class='back-to-top'
                            :class='{hide: backToTopHide}'
                            v-show='!isDashboard'></rotating-dashed-border>
  
  </div>
</template>

<script>
  import _ from 'lodash'
  import HeaderNav from 'common/HeaderNav'
  import SideBar from 'common/SideBar'
  import RotatingDashedBorder from 'styleParts/RotatingDashedBorder'
  
  export default {
    name: 'App',
    components: { RotatingDashedBorder, HeaderNav, SideBar },
    data() {
      return {
        backToTopHide: true
      }
    },
    computed: {
      isDashboard() {
        return this.$route.name === "Dashboard"
      }
    },
    mounted() {
      const scrollbarWrapper = this.$refs.scrollWrapper
      
      scrollbarWrapper.addEventListener('scroll', _.throttle(() =>
          this.backToTopHide = scrollbarWrapper.scrollTop < 100
        , 500))
    }
  }
</script>

<style rel="stylesheet/scss" lang="scss">
  @import "~styles/mixin.scss";
  
  #app {
    .content-sidebar-wrapper {
      display: flex;
      
      /* 侧边栏 */
      .sidebar-wrapper {
        z-index: 100;
      }
      
      /* 内容区 */
      .content-wrapper {
        $padding: 40px;
        width: 100%;
        height: calc(100% - #{$header-height});
        left: 0;
        padding-left: $sidebar-width;
        border-right: $padding lightblue;
        box-sizing: border-box;
        position: fixed;
        overflow-x: hidden;
        overflow-y: auto;
        margin-top: $header-height;
        
        .content {
          padding-top: 1px; // 解决margin合并的情况
          position: relative;
          padding-left: $padding;
          padding-right: $padding;
        }
      }
      
    }
    
    /* 当前路由是Dashboard的情况 */
    &.is-dashboard {
      overflow: hidden;
      .header-nav {
        display: none;
      }
      
      .content-sidebar-wrapper {
        margin-top: 0;
        overflow: hidden;
        .sidebar-wrapper {
          display: none;
        }
        .content-wrapper {
          left: 0;
          padding: 0;
          margin: 0;
          height: 100%;
          overflow: hidden;
          .content {
            margin: 0;
            overflow: hidden;
          }
        }
      }
    }
    
    .back-to-top {
      position: fixed;
      bottom: 40px;
      right: 20px;
      opacity: 1;
      visibility: visible;
      transition: visibility, transform .5s ease-out;
      
      &.hide {
        visibility: hidden;
        opacity: 0;
        transform: translate(50%);
      }
    }
  }
</style>
