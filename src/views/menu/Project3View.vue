<script lang="ts">
import {Options, Vue} from 'vue-class-component';
import {openGraphMixin} from "@/assets/ogimage/openGraphMixin";
import CurrentDate from "@/components/util/CurrentDate.vue";

@Options({
  mixins: [openGraphMixin],
  data() {
    return {
      cripView: true,
      speed: 1,
    }
  },
  mounted() {
    const mainTitle = 'Cryptocurrencies v.2.0';
    const title = 'Cryptocurrencies v.2.0';
    const metaDescription = 'Cryptocurrencies powered by CoinGecko';
    const description = 'Cryptocurrencies powered by CoinGecko';
    const imageUrl = 'https://cryptocurrencies-gold.vercel.app/assets/ogimage/bmp/project3.jpg';
    const url = 'https://cryptocurrencies-gold.vercel.app/project3';

    this.setOpenGraphTags(metaDescription, title, description, imageUrl, url);
    this.setPageTitle(mainTitle);
  },
  methods: {
    changeCrip() {
      this.cripView = !this.cripView;
    },
  },
  components: {CurrentDate},
})
export default class Project3 extends Vue {
};
</script>

<template>
  <div class="container">
    <h1>{{ $t('project3.name') }}</h1>
    <line></line>
    <CurrentDate></CurrentDate>
    <h1 class="bank"> {{ $t('h1') }}
      <a href="https://www.coingecko.com" title="In more detail..." target="_blank">
        CoinGecko
      </a>
    </h1>
<!--    <NBURatesCreepJS class="creep" :crip-view="cripView" :speed="speed"></NBURatesCreepJS>-->
  </div>
</template>

<style lang="scss" scoped>
.container {
  flex: 1 0 auto;
  background: linear-gradient(to bottom, rgb(229, 251, 255), rgb(255, 240, 244)) no-repeat center;
  h1 {font-size: 2.5rem;margin: 0.7rem auto;color: black;}

  .bank {
    font-size: 2.5rem;
    a {text-decoration: none; color: rebeccapurple;}
    a:hover {color: cornflowerblue;}
  }
  .creep {background: none;}
}
@media(max-width: 1020px) {
  .container {
    h1, .bank {font-size: 2.3rem;margin: 0.6rem auto;}
  }
}

@media (max-width: 768px) {
  .container {
    h1, .bank {font-size: 2rem;margin: 0.5rem auto;}
  }
}
</style>
