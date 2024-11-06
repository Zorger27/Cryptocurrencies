<script lang="ts">
import {Options, Vue} from 'vue-class-component';
import {openGraphMixin} from "@/assets/ogimage/openGraphMixin";
import CurrentDate from "@/components/util/CurrentDate.vue";
import Cryptocurrencies from "@/components/other/Cryptocurrencies.vue";

@Options({
  mixins: [openGraphMixin],
  data() {
    return {
      tableView: false
    }
  },
  mounted() {
    const mainTitle = 'Table / List';
    const title = 'Table / List';
    const metaDescription = 'Cryptocurrencies powered by CoinGecko';
    const description = 'Cryptocurrencies powered by CoinGecko';
    const imageUrl = 'https://cryptocurrencies-gold.vercel.app/assets/ogimage/bmp/project4.jpg';
    const url = 'https://cryptocurrencies-gold.vercel.app/project4';

    this.setOpenGraphTags(metaDescription, title, description, imageUrl, url);
    this.setPageTitle(mainTitle);
  },
  methods: {
    changeView() {
      this.tableView = !this.tableView;
    }
  },
  components: {Cryptocurrencies, CurrentDate},
})
export default class Project4 extends Vue {
};
</script>

<template>
  <div class="cryptocur">
    <h1>{{ $t('project4.name') }}</h1>
    <line></line>
    <CurrentDate></CurrentDate>
    <h1 class="bank"> {{ $t('h1') }}
      <a href="https://www.coingecko.com" title="In more detail..." target="_blank">
        CoinGecko
      </a> <i :title="[tableView ? 'Start viewing in List view' : 'Start viewing in Table view']" @click="changeView"><span :class="['fa', tableView ? 'fa-list' : 'fa-th']"></span></i>
    </h1>
    <Cryptocurrencies :table-view="tableView"></Cryptocurrencies>
  </div>
</template>

<style lang="scss" scoped>
.cryptocur {
  flex: 1 0 auto;
  background: linear-gradient(to bottom, rgb(229, 255, 229), rgb(250, 247, 234)) no-repeat center;
  h1 {font-size: 2.5rem;margin: 0.7rem auto;color: black;}

  .bank {
    font-size: 2.5rem;
    a {text-decoration: none; color: rebeccapurple;}
    a:hover {color: cornflowerblue;}
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
  }
}
</style>
