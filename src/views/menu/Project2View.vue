<script lang="ts">
import {Options, Vue} from 'vue-class-component';
import {openGraphMixin} from "@/assets/ogimage/openGraphMixin";
import CurrentDate from "@/components/util/CurrentDate.vue";
import CryptosCreep3d from "@/components/other/CryptosCreep3d.vue";

@Options({
  mixins: [openGraphMixin],
  data() {
    return {
      cripView3d: true,
    }
  },
  mounted() {
    const mainTitle = 'Cryptocurrencies 3D';
    const title = 'Cryptocurrencies 3D';
    const metaDescription = 'Cryptocurrencies powered by CoinGecko';
    const description = 'Cryptocurrencies powered by CoinGecko';
    const imageUrl = 'https://cryptocurrencies-gold.vercel.app/assets/ogimage/bmp/project2.jpg';
    const url = 'https://cryptocurrencies-gold.vercel.app/project2';

    this.setOpenGraphTags(metaDescription, title, description, imageUrl, url);
    this.setPageTitle(mainTitle);
  },
  methods: {
    changeCrip3d() {
      this.cripView3d = !this.cripView3d;
    },
  },
  components: {CryptosCreep3d, CurrentDate},
})
export default class Project2 extends Vue {
};
</script>

<template>
  <div class="container">
    <h1>{{ $t('project2.name') }}</h1>
    <line></line>
    <CurrentDate></CurrentDate>
    <h1 class="bank"> {{ $t('h1') }}
      <a href="https://www.coingecko.com" title="In more detail..." target="_blank">
        CoinGecko
      </a><i @click="changeCrip3d"><span :class="['fa', cripView3d ? 'fa-yin-yang' : 'fa-bitcoin-sign']"></span></i>
    </h1>
    <div class="creep3d">
      <CryptosCreep3d :crip-view3d="cripView3d"></CryptosCreep3d>
    </div>
  </div>
</template>

<style lang="scss" scoped>
.container {
  flex: 1 0 auto;
  background: linear-gradient(to bottom, rgb(255, 240, 244), rgb(229, 251, 255)) no-repeat center;
  h1 {font-size: 2.5rem;margin: 0.7rem auto;color: black;}

  .bank {
    font-size: 2.5rem;
    margin-bottom: 1rem;
    a {text-decoration: none; color: rebeccapurple;}
    a:hover {color: cornflowerblue;}
  }
  .creep3d {
    max-height: 20vh;
    max-width: 100%;
    position: relative;
    overflow: hidden;
    display: flex;
    align-items: center;
    justify-content: center;
    background: none;
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
