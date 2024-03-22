<script lang="ts">
import {Options, Vue} from 'vue-class-component';
import {openGraphMixin} from "@/assets/ogimage/openGraphMixin";
import CurrentDate from "@/components/util/CurrentDate.vue";
import CryptosCreep from "@/components/other/CryptosCreep.vue";
import Cryptocurrencies from "@/components/other/Cryptocurrencies.vue";
import CryptosCreep3d from "@/components/other/CryptosCreep3d.vue";

@Options({
  mixins: [openGraphMixin],
  data() {
    return {
      tableView: false,
      cripView: true,
      cripView3d: true,
      speed: 1,
    }
  },
  mounted() {
    const mainTitle = 'Cryptocurrencies Newest';
    const title = 'Cryptocurrencies Newest';
    const metaDescription = 'Cryptocurrencies powered by CoinGecko';
    const description = 'Cryptocurrencies powered by CoinGecko';
    const imageUrl = 'https://cryptocurrencies-gold.vercel.app/assets/ogimage/bmp/project1.jpg';
    const url = 'https://cryptocurrencies-gold.vercel.app';

    this.setOpenGraphTags(metaDescription, title, description, imageUrl, url);
    this.setPageTitle(mainTitle);
  },
  methods: {
    changeView() {
      this.tableView = !this.tableView;
    },
    changeCrip() {
      this.cripView = !this.cripView;
    },
    changeCrip3d() {
      this.cripView3d = !this.cripView3d;
    },
  },
  components: {CryptosCreep3d, Cryptocurrencies, CryptosCreep, CurrentDate},
})
export default class Project1 extends Vue {
};
</script>

<template>
  <div class="container">
    <h1>{{ $t('project1.name') }}</h1>
    <line></line>
    <CurrentDate></CurrentDate>
    <h1 class="bank"> {{ $t('h1') }}
      <a href="https://www.coingecko.com" title="In more detail..." target="_blank">
        CoinGecko
      </a> <i @click="changeView"><span :class="['fa', tableView ? 'fa-list' : 'fa-th']"></span></i> <i
        @click="changeCrip3d"><span :class="['fa', cripView3d ? 'fa-yin-yang' : 'fa-bitcoin-sign']"></span></i> <i
        @click="changeCrip"> <span :class="['fa', cripView ? 'fa-face-grin-stars' : 'fa-wallet']"></span></i> <input
        v-show="cripView" type="range" v-model.number="speed" min="0" max="6" step="0.2" />
    </h1>
    <div class="creep3d">
      <CryptosCreep3d :crip-view3d="cripView3d"></CryptosCreep3d>
    </div>
    <CryptosCreep class="creep" :crip-view="cripView" :speed="speed"></CryptosCreep>
    <Cryptocurrencies :table-view="tableView"></Cryptocurrencies>
  </div>
</template>

<style lang="scss" scoped>
.container {
  flex: 1 0 auto;
  background: linear-gradient(to bottom, rgb(255, 249, 229), rgb(255, 240, 244)) no-repeat center;
  h1 {font-size: 2.5rem;margin: 0.7rem auto;color: black;}

  .bank {
    font-size: 2.5rem;
    background: transparent;
    a {text-decoration: none;color: rebeccapurple;}
    a:hover {color: cornflowerblue;}
  }
  .creep3d {
    background: transparent;
    max-height: 20vh;
    max-width: 100%;
    position: relative;
    overflow: hidden;
    display: flex;
    align-items: center;
    justify-content: center;
    background: none;
  }

  .creep {
    background: transparent;
  }
}

@media(max-width: 1020px) {
  .container {
    h1, .bank {font-size: 2.3rem;margin: 0.6rem auto;}
  }
}

@media (max-width: 768px) {
  .container {
    h1, .bank {font-size: 2rem;margin: 0.5rem auto;}
    .creep3d {
      max-height: 20vh;
    }
  }
}
</style>
