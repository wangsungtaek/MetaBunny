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
    <!--START FOOTER ALTER-->
    <div class="footer-alt">
      <div class="container">
        <div class="row">
          <div class="col-lg-5">
            <div class="float-left pull-none">
              <p class="copy-rights text-muted">{{ new Date().getFullYear()}} © 330FIVE - META BUNNY PROJECT</p>
            </div>
            <div class="clearfix"></div>
          </div>
        </div>
      </div>
    </div>
    <!--START FOOTER ALTER-->
  </div>
</template>