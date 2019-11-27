<template>
  <div id="app">
    <vue-particles
      color="#fff"
      :particleOpacity="0.7"
      :particlesNumber="120"
      shapeType="circle"
      :particleSize="4"
      linesColor="#fff"
      :linesWidth="1"
      :lineLinked="true"
      :lineOpacity="0.4"
      :linesDistance="150"
      :moveSpeed="2"
      :hoverEffect="true"
      hoverMode="grab"
      :clickEffect="true"
      clickMode="push"
      class="lizi"
    >
    </vue-particles>
    <router-view v-if="isRouterAlive"></router-view>

  </div>
</template>

<script>
    export default {
        name: 'APP',
        provide() {
            return {
                  reload: this.reload
            }
        }, data() {
            return {
                isRouterAlive: true
            }
        }, methods: {
            reload() {
                this.isRouterAlive = false;
                this.$nextTick(() => {
                    this.isRouterAlive = true;
                })
            }
        }
    }
</script>

<style lang="scss">
  @import './styles/index.scss'; // 全局自定义的css样式


  #particles-js {
    /*background-color: #2d3a4b;*/
    background-image: url("assets/login/starred_hotel.jpg");
    background-repeat: no-repeat;
    background-size: 100% 100%;
    -moz-background-size: 100% 100%;
    width: 100%;
    height: 100%;
    position: fixed;
    bottom: 0px;
  }
</style>
