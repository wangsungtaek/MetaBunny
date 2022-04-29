<script>
export default {
  name: "Minting",
  
  data() {
    return {
      value: 900,
      max: 1000
    }
  }

};
</script>

<template>
  <div>
    <div class="account-home-btn d-none d-sm-block">
      <a href="/" class="text-white">
        <i class="mdi mdi-home h1"></i>

      </a>
    </div>
    <section class="bg-account-pages height-100vh">
      <div class="display-table">
        <div class="display-table-cell">
          <div class="container">
            <div class="row justify-content-center">
              <div class="col-lg-12">
                <div class="card account-card">
                  <div class="card-body">

                    
                    <div class="text-center mt-3">
                      <h3 class="font-weight-bold">
                        <a href="/" class="text-dark text-uppercase account-pages-logo">META BUNNY</a>
                      </h3>
                      <p class="text-muted">Minting</p>
                    </div>

                    <div class="row justify-content-center pt-5">
                      <div class="col-lg-7 text-center">
                        <img src="@/assets/images/bg-home.jpg" alt style="width: 400px">
                      </div>
                      <div class="col-lg-5">
                        <div class="row">
                          <div class="col-lg-12 title">
                            Minting Block Number
                          </div>
                        </div>
                        <div class="row mt-3">
                          <div class="col-lg-6">
                            CURRENT BLOCK
                          </div>
                          <h3 class="col-lg-6 font-weight-bold">
                            #8312344
                          </h3>
                        </div>

                        <div class="row mt-2">
                          <div class="col-lg-6">
                            MINTING START AT
                          </div>
                          <h3 class="col-lg-6 font-weight-bold">
                            #8315333
                          </h3>
                        </div>

                        <div class="row mt-4">
                          <div class="col-lg-4">
                            <div class="title">Price</div>
                            <img src="@/assets/images/klay.png" alt style="width: 18px;">
                            300 KLAY
                          </div>
                          <div class="col-log-4">
                            <div class="title">Per Transaction</div>
                            최대 <Strong>100개</Strong>
                          </div>
                        </div>

                        <div class="row mt-5">
                          <div class="col-lg-10">
                          <h5 class="title">NFT 판매 수량</h5>
                          <b-progress :max="max" height="2rem">
                            <b-progress-bar :value="value">
                              <span>판매수량: <strong>{{ value }} / {{ max }}</strong></span>
                            </b-progress-bar>
                          </b-progress>
                          </div>
                        </div>

                        <div class="row mt-5">
                          <div class="col-lg-12 title">
                            Amount
                          </div>
                        </div>

                        <div class="row mt-2">
                          <div class="col-lg-12">
                            <input/>
                          </div>
                        </div>

                        <div class="row mt-5">
                          <div class="col-lg-10 text-center">
                            <b-button variant="success" style="width: 100%">Button</b-button>
                          </div>
                        </div>

                      </div>
                    </div>
                  </div>
                </div>
              </div>
              
            </div>

          </div>
        </div>
      </div>
    </section>
    <!-- end account-pages  -->
  </div>
</template>
<style scoped>
.title {
  color: red;
}
</style>