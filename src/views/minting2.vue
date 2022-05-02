<script>

export default {
  name: "Minting",
  
  created() {
  },

  data() {
    return {
      
      klaytn: '',

      value: 0,
      max: 0,

      mintingNumber: 0,

      isConnect: false,
      walletAdress: '',
      myKlay: '',

      // 퍼블릭 민팅
      setupSale: {
        mintIndexForSale: '',
        mintLimitPerBlock: '',
        mintStartBlockNumber: '',
        maxSaleAmount: '',
        mintPrice: ''
      },

      blockNumber: '',
      blockCnt: false,

    }
  },

  methods: {
    
    // 지갑연동
    async connect() {
      this.klaytn = window.klaytn;
      const accounts = await this.klaytn.enable();
      const account = accounts[0];
      
      if(account != null) {
        this.walletAdress = account
        this.isConnect = true
      }

      this.check_status()
    },

    // 트랜잭션 민팅 상태 확인
    async check_status() {
      
    },

    // BlockCount
    cntBlockNumber() {
      if(!this.blockCnt) {
        setInterval(() => {
          this.blockNumber += 1;
        }, 1000);
        this.blockCnt = true;
      } 

    },

    minus() {
      if(this.mintingNumber > 0) {
        this.mintingNumber -= 1;
      }
    },

    plus() {
      if(this.mintingNumber < this.setupSale.mintLimitPerBlock) {
        this.mintingNumber += 1;
      }
    },

    // 민팅
    async publicMint() {
      
      const transactionParameters = {
        to: "0x1aA4a2315fa73328c0873E04060FF8F640413A29",
        from: this.walletAdress,
        data: "0xa0712d680000000000000000000000000000000000000000000000000000000000000001",
        value: "0xB1A2BC2EC50000", // 0.05 Klay
        gas: "0x3476A",
      };

      // 하고 싶은 일 블록체인에 요청하기
      this.klaytn.sendAsync(
        {
          method: "klay_sendTransaction",
          params: [transactionParameters],
          from: this.account
        },
        (receipt, result) => {
          console.log(receipt);
          console.log(result);
        }
      );

  
    }   

  }

};
</script>

<template>
  <div>
    <div class="account-home-btn d-none d-sm-block">
      <!-- <a href="/" class="text-white">
        <i class="mdi mdi-home h1"></i>
      </a> -->
      <div class="row justify-content-end">
        <div class="col-lg-12">
          <b-button v-if="!isConnect" variant="outline-white text-white font-weight-bold" @click="connect">지갑연결</b-button>
          <div id="wallet" v-if="isConnect">
            <img v-if="isConnect" src="@/assets/images/klay.png" alt style="width: 20px;" id="klayLogo" class="">
            <span v-if="isConnect" class="text-white">{{ walletAdress }}</span>
            <!-- <div v-if="isConnect" class="text-white">{{ myKlay }}</div> -->
          </div>
        </div>
      </div>
    </div>
    <section class="bg-account-pages height-100vh">
      <div class="display-table">
        <div class="display-table-cell">
          <div class="container">
            <div class="row justify-content-center">
              <div class="col-lg-12">
                <!-- <div class="card account-card">
                  <div class="card-body"> -->

                    
                    <div class="text-center mt-3">
                      <h1 class="font-weight-bold">
                        <a href="/" class="text-white text-uppercase account-pages-logo">META BUNNY</a>
                      </h1>
                      <p class="text-white">Minting</p>
                    </div>

                    <div class="row justify-content-center align-content-center pt-5">

                      <!-- image -->
                      <div class="col-lg-6 align-content-center">
                        <img src="@/assets/images/minting.png" alt style="width: 450px">
                      </div>

                      <!-- Block -->
                      <div class="col-lg-6 justfy-content-end">

                        <div class="row item">
                          <div class="col-lg-6 align-content-center justify-content-center font-weight-bold pt-1">
                            CURRENT BLOCK
                          </div>
                          <h3 class="col-lg-6 font-weight-bold">
                            {{ `#${blockNumber}` }}
                          </h3>
                        </div>

                        <div class="row mt-2 item">
                          <div class="col-lg-6 align-content-center font-weight-bold pt-1">
                            MINTING START AT
                          </div>
                          <h3 class="col-lg-6 font-weight-bold">
                            {{ `#${setupSale.mintStartBlockNumber}` }}
                          </h3>
                        </div>

                        <div class="row mt-2 item">
                          <div class="col-lg-6 align-content-center font-weight-bold pt-1">
                            PRICE
                          </div>
                          <h3 class="col-lg-6 font-weight-bold">
                            <!-- <img src="@/assets/images/klay.png" alt style="width: 20px;" id="klayLogo" class=""> -->
                            {{ setupSale.mintPrice }} KLAY
                          </h3>
                        </div>

                        <div class="row mt-2 item">
                          <div class="col-lg-6 align-content-center font-weight-bold pt-1">
                            PER TRANSACTION
                          </div>
                          <h3 class="col-lg-6 font-weight-bold">
                            <!-- <img src="@/assets/images/klay.png" alt style="width: 20px;" id="klayLogo" class=""> -->
                            {{ setupSale.mintLimitPerBlock }}
                          </h3>
                        </div>

                        <div class="row mt-2 item">
                          <div class="col-lg-6 align-content-center font-weight-bold pt-1">
                            판매수량
                          </div>
                          <h3 class="col-lg-6 font-weight-bold">
                            <!-- <img src="@/assets/images/klay.png" alt style="width: 20px;" id="klayLogo" class=""> -->
                            {{ setupSale.mintIndexForSale }} / {{ setupSale.maxSaleAmount }}
                          </h3>
                        </div>

                        <div class="row amount align-items-center mb-4">
                          <div class="col-lg-2">
                            <b-button variant="dark" @click="minus">-</b-button>
                          </div>
                          <div class="col-lg-6 text-center">
                            <b-form-input type="number" max="3" disabled v-model="mintingNumber" class="text-center font-weight-bold mb-0" id="amountInput"></b-form-input>
                          </div>
                          <div class="col-lg-2 text-right">
                            <b-button variant="dark" @click="plus">+</b-button>
                          </div>
                        </div>

                        <div class="row mt-1">
                          <div class="col-lg-10 text-center">
                            <b-button variant="danger" style="width: 100%" :disabled="!isConnect" @click="publicMint">
                              MINTING
                            </b-button>
                          </div>
                        </div>

                      </div>
                    </div>
                  </div>
                <!-- </div>
              </div> -->
              
            </div>

          </div>
        </div>
      </div>
    </section>
    <!-- end account-pages  -->
  </div>
</template>
<style scoped>
#wallet {
  width: 150px;
  overflow: hidden;
  border: 2px solid white;
  border-radius: 18px;
  height: 39px;
}
#wallet img {
  width: 20px;
  position: absolute;
  top: 10px;
  left: 25px;
}
#wallet span {
  position: absolute;
  top: 7px;
  left: 53px;
  width: 95px;
  overflow: hidden;
}
.title {
  color: white;
}
.bg-account-pages {
  background: linear-gradient(#e66465, #9198e5);
}
.amount {
  margin-top: 90px;
}
.item {
  height: 40px;
}
#amountInput {
  font-size: 25px;
}
</style>