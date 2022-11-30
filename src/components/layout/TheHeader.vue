<template>
    <div class="header">
        <div class="header-left">
          <div class="header-left__toggle">
            <i class="icofont-navigation-menu"></i>
          </div>
          <div class="header-left__logo">
            <div class="header-left__logo_img">
              <img
                src="../../assets/icofont/favicon.jpg"
                width="32px"
                height="32px"
              />
            </div>
            <div class="header-left__logo_text">KẾ TOÁN</div>
          </div>
        </div>
        <div class="header-right">
          <div class="header-right__home">
            <div class="header-right__toggle">
              <i class="icofont-navigation-menu"></i>
            </div>
            <div class="header-right__name">
              CÔNG TY CỔ PHẦN MISA <i class="icofont-rounded-down"></i>
            </div>
          </div>
          <div class="header-right__feature">
            <div class="header-right__slogan">
              <i class="icofont-notification"></i>
            </div>
            <div class="header-right__avatar">
              <img src="../../libs/meme.jpg"/>
            </div>
            <div class="header-right__name">
              <span>Nguyễn Văn Mạnh <i class="icofont-rounded-down"></i></span>
            </div>
          </div>
        </div>
      </div>
</template>
<script>
export default {
  name:"TheHeader",
  components:"",
  props:[],
  methods: {
    
  },
  data() {
    return {
      
    }
  },

}
</script>
<style scoped>
  @import url("../../css/layout/header.css");
  @import url("../../assets/icofont/icofont.css");
</style>