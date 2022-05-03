<script>
import Navbar from "@/components/navbar";

import Footer from "@/components/footer";

import MetaBunny from '@/components/MetaBunny.vue';
import Kkuppae from "@/components/Kkuppae";
import BucketList from '@/components/BucketList.vue';
import NFT from '@/components/NFT.vue';
import RoadMap from '@/components/RoadMap.vue';
import Team330 from '@/components/Team330.vue';
import benefits from '../components/benefits.vue';


export default {
  name: "index-1",
  components: {
    Navbar,
    Kkuppae,
    Footer,
    MetaBunny,
    BucketList,
    NFT,
    RoadMap,
    Team330,
    benefits
  },
};
</script>

<template>
  <div>
    <Navbar />
    <div v-scroll-spy>
      <!--START HOME-->
      <section
        class="section bg-home home-half"
        id="home"
        :style="{'background-image': 'url(' + require('@/assets/images/bg-home.jpg') + ')', 'background-size': 'cover','background-position': 'center', 'width': '500' }"
      >
        <div class="bg-overlay"></div>
        <div class="display-table">
          <div class="display-table-cell">
            <div class="container">
              <div class="row">
                <div class="col-lg-8 offset-lg-2 text-white text-center">
                  <h1 class="home-title">진짜 행복이란 무엇일까 ?</h1>
                  <p
                    class="padding-t-15 home-desc"
                  >행복은 성장하는데 찾아옵니다. META BUNNY와 함께 버니랜드로 떠나보세요!</p>
                </div>
              </div>
            </div>
          </div>
        </div>
        <div class="wave-effect wave-anim">
          <div class="waves-shape shape-one">
            <div
              class="wave wave-one"
              :style="{'background-image': 'url('+ require('@/assets/images/wave-shape/wave1.png')+ ')'}"
            ></div>
          </div>
          <div class="waves-shape shape-two">
            <div
              class="wave wave-two"
              :style="{'background-image': 'url('+ require('@/assets/images/wave-shape/wave2.png')+ ')'}"
            ></div>
          </div>
          <div class="waves-shape shape-three">
            <div
              class="wave wave-three"
              :style="{'background-image': 'url('+ require('@/assets/images/wave-shape/wave3.png')+ ')'}"
            ></div>
          </div>
        </div>
      </section>
      <!--END HOME-->
      <Kkuppae />
      <MetaBunny />
      <BucketList />
      <NFT />
      <RoadMap />
      <benefits />
      <Team330 />
      <Footer />
    </div>
  </div>
</template>

<style scoped>
::v-deep .modal-content {
  background-color: #000000;
  color: white;
  background: none;
  border: none;
}
::v-deep .modal-header {
  border: none;
}
::v-deep .modal-backdrop {
  opacity: 0.5;
}
</style>