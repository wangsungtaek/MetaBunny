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
        mintStartBlockNumber: '',
        maxSaleAmount: '',
        mintPrice: ''
      },

      blockNumber: '',
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
      this.setupSale.mintPrice = `${window.caver.utils.fromPeb(parseInt(result[6]), "KLAY")}` // 민팅 가격
      

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
      if(this.mintingNumber < this.setupSale.mintLimitPerBlock) {
        this.mintingNumber += 1;
      }
    },

    // 민팅
    async publicMint() {
    if (window.klaytn.networkVersion === 8217) {
        console.log("메인넷");
    } else if (window.klaytn.networkVersion === 1001) {
        console.log("테스트넷");
    } else {
        alert("ERROR: 클레이튼 네트워크로 연결되지 않았습니다!");
        return;
    }
    if (!this.isConnect) {
        alert("ERROR: 지갑을 연결해주세요!");
        return;
    }

    const myContract = new window.caver.klay.Contract(this.ABI, this.CONTRACTADDRESS);
    const amount = this.mintingNumber;
    await this.check_status();

    if (this.setupSale.maxSaleAmount + 1 <= this.setupSale.mintIndexForSale) {
        alert("모든 물량이 소진되었습니다.");
        return;
    } else if (this.blockNumber <= this.setupSale.mintStartBlockNumber) {
        alert("아직 민팅이 시작되지 않았습니다.");
        return;
    }
    const total_value = amount * this.setupSale.mintPrice;

    // let estmated_gas;

    console.log('myContract : ', myContract)
    console.log('amount : ', amount)
    console.log('this.setupSale.mintPrice : ', this.setupSale.mintPrice)
    console.log(total_value)

    if(amount <= 0) {
      alert('민팅 수량을 입력해주세요.')
      return
    }

    const accounts = await window.klaytn.enable();
    const account = accounts[0];

    console.log(account);
    console.log(this.walletAdress);
    
    // 하고 싶은 일
    // const transactionParameters = {
    //   to: this.walletAdress,
    //   from: account,
    //   data: "0x2db115440000000000000000000000000000000000000000000000000000000000000001"
    // };

  // // 블록체인에 실제 물어보기
  // window.klaytn.sendAsync({
  //   method: "publicMint",
  //   params: [transactionParameters, "latest"],
  //   from: account
  // },
  //   (receipt, result) => {
  //     console.log(result);
  //   }
  // );


    const result = await myContract.methods.publicMint(amount)
            .estimateGas({
              from: account,
              gas: 1000,
              value: total_value
            })
    console.log(result)
        // .estimateGas({
        //     from: this.walletAdress,
        //     gas: 6000000,
        //     value: total_value
        // })
        // .then( (gasAmount) => {
        //     console.log('gasAmount: ', gasAmount)
        //     estmated_gas = gasAmount;
        //     console.log("gas :" + estmated_gas);
        //     myContract.methods.publicMint(amount)
        //         .send({
        //             from: this.walletAdress,
        //             gas: estmated_gas,
        //             value: total_value
        //         })
        //         .on("transactionHash", (txid) => {
        //             console.log(txid);
        //         })
        //         .once("allEvents", (allEvents) => {
        //             console.log(allEvents);
        //         })
        //         .once("Transfer", (transferEvent) => {
        //             console.log(transferEvent);
        //         })
        //         .once("receipt", (receipt) => {
        //             console.log(receipt)
        //             alert("민팅에 성공하였습니다.");
        //         })
        //         .on("error", (error) => {
        //             alert("민팅에 실패하였습니다.");
        //             console.log(error);
        //         });
        // })
        // .catch(function (error) {
        //     console.log(error);
        //     alert("민팅에 실패하였습니다.");
        // });
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
                        <div class="row align-content-center justify-content-center">
                          <div class="col-lg-6 align-content-center justify-content-center">
                            CURRENT BLOCK
                          </div>
                          <h3 class="col-lg-6 font-weight-bold">
                            {{ `#${blockNumber}` }}
                          </h3>
                        </div>

                        <div class="row mt-2">
                          <div class="col-lg-6 align-content-center">
                            MINTING START AT
                          </div>
                          <h3 class="col-lg-6 font-weight-bold">
                            {{ `#${setupSale.mintStartBlockNumber}` }}
                          </h3>
                        </div>

                        <div class="row mt-2">
                          <div class="col-lg-6 align-content-center">
                            PRICE
                          </div>
                          <h3 class="col-lg-6 font-weight-bold">
                            <!-- <img src="@/assets/images/klay.png" alt style="width: 20px;" id="klayLogo" class=""> -->
                            {{ setupSale.mintPrice }} KLAY
                          </h3>
                        </div>

                        <div class="row mt-2">
                          <div class="col-lg-6 align-content-center">
                            PER TRANSACTION
                          </div>
                          <h3 class="col-lg-6 font-weight-bold">
                            <!-- <img src="@/assets/images/klay.png" alt style="width: 20px;" id="klayLogo" class=""> -->
                            {{ setupSale.mintLimitPerBlock }}
                          </h3>
                        </div>

                        <div class="row mt-2">
                          <div class="col-lg-6 align-content-center">
                            판매수량
                          </div>
                          <h3 class="col-lg-6 font-weight-bold">
                            <!-- <img src="@/assets/images/klay.png" alt style="width: 20px;" id="klayLogo" class=""> -->
                            {{ setupSale.mintLimitPerBlock }}
                          </h3>
                        </div>

                        <div class="row mt-5">
                          <div class="col-lg-10">
                          <h5 class="title">NFT 판매 수량</h5>
                          <b-progress :max="setupSale.maxSaleAmount" height="2rem">
                            <b-progress-bar :value="setupSale.mintIndexForSale">
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
                            <b-button variant="success" style="width: 100%" :disabled="!isConnect" @click="publicMint">
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
.title {
  color: white;
}
.bg-account-pages {
  background: linear-gradient(#e66465, #9198e5);
}
#klayLogo {
  position: absolute;
  
}
</style>