<script>
import config from '../../config.js'

export default {
  name: "Minting",
  
  created() {
  },

  data() {
    return {
      ...config,
      
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
        mintStartBlockNumber: '#0',
        maxSaleAmount: '',
        mintPrice: '00'
      },

      blockNumber: 0,
      blockCnt: false

    }
  },

  methods: {
    
    // 지갑연동
    async connect() {

        const accounts = await window.klaytn.enable();
        
        if (window.klaytn.networkVersion === 8217) {
            console.log("메인넷");
        } else if (window.klaytn.networkVersion === 1001) {
            console.log("테스트넷");
        } else {
            alert("ERROR: 클레이튼 네트워크로 연결되지 않았습니다!");
            return;
        }
        this.isConnect = true;
        
        const account = accounts[0];
        const balance = await window.caver.klay.getBalance(account)
        this.walletAdress = account;
        this.myKlay = `잔액: ${window.caver.utils.fromPeb(balance, "KLAY")} KLAY`

            // .then(function (balance) {

            //     self.walletAdress = `지갑주소: ${account}`
            //     self.myKlay = `잔액: ${window.caver.utils.fromPeb(balance, "KLAY")} KLAY`
            // });
        this.check_status();
    },

    // 트랜잭션 민팅 상태 확인
    async check_status() {

      const myContract = new window.caver.klay.Contract(this.ABI, this.CONTRACTADDRESS);

      const result = await myContract.methods.mintingInformation().call()
      console.log(result);
      
      this.setupSale.mintIndexForSale = parseInt(result[1]); // 민팅 시작 Index
      this.setupSale.mintLimitPerBlock = parseInt(result[2]); // 트랙잭션당 최대 수량
      this.setupSale.mintStartBlockNumber = parseInt(result[4]); // 민팅 시작 블록
      this.setupSale.maxSaleAmount = parseInt(result[5]); // 최대 발행
      this.setupSale.mintPrice = `${window.caver.utils.fromPeb(parseInt(result[6]), "KLAY")} KLAY` // 민팅 가격
      

      const blockNumber = await window.caver.klay.getBlockNumber();
      this.blockNumber = blockNumber
      this.cntBlockNumber();
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
      if(this.mintingNumber < this.setupSale.maxSaleAmount) {
        this.mintingNumber += 1;
      }
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
      <b-button v-if="!isConnect" variant="outline-white text-white" @click="connect">지갑연결</b-button>
      <div v-if="isConnect" class="text-muted">{{ walletAdress }}</div>
      <div v-if="isConnect" class="text-muted">{{ myKlay }}</div>
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
                            #{{ blockNumber }}
                          </h3>
                        </div>

                        <div class="row mt-2">
                          <div class="col-lg-6">
                            MINTING START AT
                          </div>
                          <h3 class="col-lg-6 font-weight-bold">
                            #{{ setupSale.mintStartBlockNumber }}
                          </h3>
                        </div>

                        <div class="row mt-4">
                          <div class="col-lg-4">
                            <div class="title">Price</div>
                            <img src="@/assets/images/klay.png" alt style="width: 18px;">
                            {{ setupSale.mintPrice }}
                          </div>
                          <div class="col-log-4">
                            <div class="title">Per Transaction</div>
                            최대 <Strong>{{ setupSale.maxSaleAmount }}</Strong>
                          </div>
                        </div>

                        <div class="row mt-5">
                          <div class="col-lg-10">
                          <h5 class="title">NFT 판매 수량</h5>
                          <b-progress :max="max" height="2rem">
                            <b-progress-bar :value="value">
                              <span>판매수량: <strong>{{ setupSale.mintIndexForSale }} / {{ setupSale.maxSaleAmount }}</strong></span>
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
                          <div class="col-lg-2">
                            <b-button @click="minus">-</b-button>
                          </div>
                          <div class="col-lg-6">
                            <b-form-input type="number" max="3" disabled v-model="mintingNumber" class="text-center font-weight-bold"></b-form-input>
                          </div>
                          <div class="col-lg-2">
                            <b-button @click="plus">+</b-button>
                          </div>
                        </div>

                        <div class="row mt-1">
                          <div class="col-lg-10 text-center">
                            <b-button variant="success" style="width: 100%">MINTING</b-button>
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
  color: rgb(219, 125, 125);
}
</style>