<script>
export default {
  data() {
    return {
      isVisible: false,
    };
  },
  methods: {
    toggleSwitcher() {
      this.isVisible = !this.isVisible;
    },

    /**
     * Onclick color change
     * @param theme theme color
     */
    setTheme(theme) {
      document
        .getElementById("color-opt")
        .setAttribute("href", "./css/colors/" + theme + ".css");
    },
  },
};
</script>


<template>
  <div>
    <!--START FOOTER-->
    <footer class="footer">
      <div class="container">
        <div class="row">
          <div class="col-lg-3 margin-t-20">
            <h4>DORSIN</h4>
            <div class="text-muted margin-t-20">
              <ul class="list-unstyled footer-list">
                <li>
                  <a href="javascript: void(0);">Home</a>
                </li>
                <li>
                  <a href="javascript: void(0);">About us</a>
                </li>
                <li>
                  <a href="javascript: void(0);">Careers</a>
                </li>
                <li>
                  <a href="javascript: void(0);">Contact us</a>
                </li>
              </ul>
            </div>
          </div>
          <div class="col-lg-3 margin-t-20">
            <h4>Information</h4>
            <div class="text-muted margin-t-20">
              <ul class="list-unstyled footer-list">
                <li>
                  <a href="javascript: void(0);">Terms & Condition</a>
                </li>
                <li>
                  <a href="javascript: void(0);">About us</a>
                </li>
                <li>
                  <a href="javascript: void(0);">Jobs</a>
                </li>
                <li>
                  <a href="javascript: void(0);">Bookmarks</a>
                </li>
              </ul>
            </div>
          </div>
          <div class="col-lg-3 margin-t-20">
            <h4>Support</h4>
            <div class="text-muted margin-t-20">
              <ul class="list-unstyled footer-list">
                <li>
                  <a href>FAQ</a>
                </li>
                <li>
                  <a href>Contact</a>
                </li>
                <li>
                  <a href>Disscusion</a>
                </li>
              </ul>
            </div>
          </div>
          <div class="col-lg-3 margin-t-20">
            <h4>Subscribe</h4>
            <div class="text-muted margin-t-20">
              <p>In an ideal world this text wouldn’t exist, a client would acknowledge the importance of having web copy before the design starts.</p>
            </div>
            <form class="form subscribe">
              <input placeholder="Email" class="form-control" required />
              <a href="javascript: void(0);" class="submit">
                <i class="pe-7s-paper-plane"></i>
              </a>
            </form>
          </div>
        </div>
      </div>
    </footer>
    <!--END FOOTER-->

    <!--START FOOTER ALTER-->
    <div class="footer-alt">
      <div class="container">
        <div class="row">
          <div class="col-lg-12">
            <div class="float-left pull-none">
              <p class="copy-rights text-muted">{{ new Date().getFullYear()}} © Dorsin - Themesbrand</p>
            </div>
            <div class="float-right pull-none">
              <img src="@/assets/images/payment.png" alt="payment-img" height="36" />
            </div>
            <div class="clearfix"></div>
          </div>
        </div>
      </div>
    </div>
    <!--START FOOTER ALTER-->
    <!-- Style switcher -->
    <div id="style-switcher" :style="isVisible ? 'left: 0px' : 'left: -189px'">
      <div>
        <h3>Select your color</h3>
        <ul class="pattern">
          <li>
            <a class="color1" href="javascript: void(0);" v-on:click="setTheme('red')"></a>
          </li>
          <li>
            <a class="color2" href="javascript: void(0);" v-on:click="setTheme('green')"></a>
          </li>
          <li>
            <a class="color3" href="javascript: void(0);" v-on:click="setTheme('pink')"></a>
          </li>
          <li>
            <a class="color4" href="javascript: void(0);" v-on:click="setTheme('cyan')"></a>
          </li>
          <li>
            <a class="color5" href="javascript: void(0);" v-on:click="setTheme('purple')"></a>
          </li>
          <li>
            <a class="color6" href="javascript: void(0);" v-on:click="setTheme('blue')"></a>
          </li>
          <li>
            <a class="color7" href="javascript: void(0);" v-on:click="setTheme('orange')"></a>
          </li>
          <li>
            <a class="color8" href="javascript: void(0);" v-on:click="setTheme('yellow')"></a>
          </li>
        </ul>
      </div>
      <div class="bottom">
        <a href="javascript: void(0);" class="settings" @click="toggleSwitcher()">
          <i class="mdi mdi-settings mdi-spin"></i>
        </a>
      </div>
    </div>
    <!-- end Style switcher -->
  </div>
</template>