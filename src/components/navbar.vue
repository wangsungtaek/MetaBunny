<script>
export default {
  mounted: () => {
    window.onscroll = function () {
      onwindowScroll();
    };
    var navbar = document.getElementById("navbar");
    function onwindowScroll() {
      if (
        document.body.scrollTop > 50 ||
        document.documentElement.scrollTop > 50
      ) {
        navbar.style.backgroundColor = "#272a33";
        navbar.style.padding = "10px";
      } else {
        navbar.style.backgroundColor = "";
        navbar.style.padding = "20px";
      }
    }
  },
  methods: {
    /**
     * Toggle menu
     */
    toggleMenu() {
      console.log(document.getElementById("navbarCollapse"));
      document.getElementById("navbarCollapse").classList.toggle("show");
      
    },
  },
};
</script>

<template>
  <!--Navbar Start-->
  <nav class="navbar navbar-expand-lg fixed-top navbar-custom sticky sticky-dark" id="navbar">
    <div class="container">
      <!-- LOGO -->
      <a class="navbar-brand logo text-uppercase" href="/">META BUNNY</a>
      <button
        class="navbar-toggler"
        type="button"
        data-toggle="collapse"
        data-target="#navbarCollapse"
        aria-controls="navbarCollapse"
        aria-expanded="false"
        aria-label="Toggle navigation"
        @click="toggleMenu()"
      >
        <i class="mdi mdi-menu"></i>
      </button>
      <div class="collapse navbar-collapse" id="navbarCollapse">
        <ul class="navbar-nav ml-auto navbar-center" id="mySidenav" v-scroll-spy-active>
          <li class="nav-item">
            <a v-scroll-to="'#home'" href="javascript: void(0);" class="nav-link">홈</a>
          </li>
          <li class="nav-item">
            <a v-scroll-to="'#kkuppa'" href="javascript: void(0);" class="nav-link">스토리</a>
          </li>
          <li class="nav-item">
            <a v-scroll-to="'#bucketList'" href="javascript: void(0);" class="nav-link">버킷리스트</a>
          </li>
          <li class="nav-item">
            <a v-scroll-to="'#nft'" href="javascript: void(0);" class="nav-link">NFT</a>
          </li>
          <li class="nav-item">
            <a v-scroll-to="'#roadMap'" href="javascript: void(0);" class="nav-link">로드맵</a>
          </li>
          <li class="nav-item">
            <a v-scroll-to="'#team'" href="javascript: void(0);" class="nav-link">팀</a>
          </li>
          <li class="nav-item">
            <a href="/minting" class="nav-link">민팅</a>
          </li> 

        </ul>
        <div class="nav-button ml-auto">
          <ul class="nav navbar-nav navbar-right">
            <li>
              <a href="https://discord.gg/zTQmTrtp" class="waves-light waves-effect margin-t-10">
                <img src="@/assets/images/icon_discord.png" class="link">
              </a>
            </li>
            <li>
              <a href="https://twitter.com/metabunny0330" class="waves-light waves-effect margin-t-10">
                <img src="@/assets/images/icon_twitter.png" class="link">
              </a>
            </li>
            <li>
              <a href="http://naver.com" class="waves-light waves-effect margin-t-10">
                <img src="@/assets/images/icon_kakao.png" class="link">
              </a>
            </li>
            
            
          </ul>
        </div>
      </div>
    </div>
  </nav>
  <!-- Navbar End -->
</template>
<style scoped>
.link {
  width: 25px;
}
.link:hover {
  color: black;
}
</style>