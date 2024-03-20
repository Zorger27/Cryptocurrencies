<script lang="ts">
import {Options, Vue} from "vue-class-component";
import axios from "axios";

interface CryptoResponse {
  [id: string]: {
    usd: number;
  };
}
@Options({
  data() {
    return {
      cryptos: [
        { id: 'bitcoin', name: 'Bitcoin', url: 'https://www.coingecko.com/en/coins/bitcoin', price: 0 },
        { id: 'ethereum', name: 'Ethereum', url: 'https://www.coingecko.com/en/coins/ethereum', price: 0 },
        { id: 'litecoin', name: 'Litecoin', url: 'https://www.coingecko.com/en/coins/litecoin', price: 0 },
        { id: 'tether', name: 'Tether', url: 'https://www.coingecko.com/en/coins/tether', price: 0 },
        { id: 'avalanche-2', name: 'Avalanche', url: 'https://www.coingecko.com/en/coins/avalanche', price: 0 },
        { id: 'filecoin', name: 'Filecoin', url: 'https://www.coingecko.com/en/coins/filecoin', price: 0 },
        { id: 'bitcoin-cash', name: 'Bitcoin Cash', url: 'https://www.coingecko.com/en/coins/bitcoin-cash', price: 0 },
        { id: 'binancecoin', name: 'BNB', url: 'https://www.coingecko.com/en/coins/bnb', price: 0 },
        { id: 'dogecoin', name: 'Dogecoin', url: 'https://www.coingecko.com/en/coins/dogecoin', price: 0 },
        { id: 'ripple', name: 'XRP', url: 'https://www.coingecko.com/en/coins/ripple', price: 0 },
        { id: 'cardano', name: 'Cardano', url: 'https://www.coingecko.com/en/coins/cardano', price: 0 },
        { id: 'polkadot', name: 'Polkadot', url: 'https://www.coingecko.com/en/coins/polkadot', price: 0 },
        { id: 'chainlink', name: 'Chainlink', url: 'https://www.coingecko.com/en/coins/chainlink', price: 0 },
        { id: 'stellar', name: 'Stellar', url: 'https://www.coingecko.com/en/coins/stellar', price: 0 },
      ],
      contentWidth: 0,
      marqueeWidth: 0,
      animationOffset: 0,
      animationInterval: null as any, // 'null as any' для инициализации с поддержкой TypeScript
    };
  },
  mounted() {
    this.fetchCryptoRates().then(() => {
      this.$nextTick(() => {
        const content = this.$refs.marquee.querySelector('.content');
        if (content) {
          // Дублируем содержимое
          const contentHTML = content.innerHTML;
          content.innerHTML += contentHTML;
          // Пересчитываем ширину после дублирования
          this.calculateWidths();
        }
      });
    });
  },
  beforeUnmount() {
    if (this.animationInterval) {
      clearInterval(this.animationInterval);
    }
    window.removeEventListener("resize", this.calculateWidths);
  },
  watch: {
    speed(newSpeed: number) {
      // Обрабатываем изменение скорости
      this.updateAnimationSpeed(newSpeed);
    }
  },
  methods: {
    async fetchCryptoRates() {
      try {
        const response = await axios.get<CryptoResponse>('https://api.coingecko.com/api/v3/simple/price?ids=bitcoin,ethereum,litecoin,tether,avalanche-2,filecoin,bitcoin-cash,binancecoin,dogecoin,ripple,cardano,polkadot,chainlink,stellar&vs_currencies=usd');
        this.cryptos = Object.entries(response.data).map(([id, { usd }]: [string, { usd: number }]) => {
          const cryptoInfo = this.cryptos.find((item: { id: string; name: string; url: string; price: number; }) => item.id === id);
          return {
            id: id,
            price: usd,
            name: cryptoInfo ? cryptoInfo.name : '',
            url: cryptoInfo ? cryptoInfo.url : '',
          };
        });

        await this.$nextTick(this.calculateWidths);
        this.setupAnimationListener();
      } catch (error) {
        console.error(error);
      }
    },
    calculateWidths() {
      const marquee = this.$refs.marquee as HTMLElement | null;
      const content = marquee?.querySelector(".content") as HTMLElement | null;
      if (marquee && content) {
        this.marqueeWidth = marquee.offsetWidth;
        this.contentWidth = content.offsetWidth;
        // Устанавливаем начальное смещение таким образом, чтобы вторая копия была частично видима в конце первой копии
        this.animationOffset = this.contentWidth;
      }
    },
    setupAnimationListener() {
      window.addEventListener("resize", this.calculateWidths);
      this.$nextTick(this.calculateWidths);
      this.animateMarquee();
    },
    animateMarquee() {
      if (this.animationInterval) {
        clearInterval(this.animationInterval);
      }
      this.animationInterval = setInterval(() => {
        // Проверяем, проскроллена ли полностью первая копия содержимого
        if (this.animationOffset >= this.contentWidth) {
          // Если да, сбрасываем смещение на начало
          this.animationOffset = 0;
        } else {
          // Иначе продолжаем анимацию
          this.animationOffset += this.speed;
        }

        const content = this.$refs.marquee?.querySelector(".content");
        if (content) {
          content.style.transform = `translateX(${-this.animationOffset}px)`;
        }
      }, 10);
    },
    updateAnimationSpeed(newSpeed: number) {
      // Проверяем, что speed действительно изменился, прежде чем обновлять анимацию
      if (this.speed !== newSpeed) {
        this.speed = newSpeed; // Обновляем скорость в состоянии компонента
        clearInterval(this.animationInterval); // Очищаем текущий интервал
        this.animateMarquee(); // Запускаем анимацию с новой скоростью
      }
    },
  },
  props: {
    cripView: {
      type: Boolean,
      required: true
    },
    speed: {
      type: Number,
      default: 1,
    },
  },
  components: {},
})
export default class CryptosCreep extends Vue {}
</script>

<template>
  <div class="inner" v-show="cripView">
    <div ref="marquee" class="marquee">
      <div class="content">
        <div v-for="(crypto) in cryptos" :key="crypto.id" class="crypto">
          <span class="name">{{ crypto.name }}</span><span class="eql">=</span><span class="price">{{ crypto.price }}</span><span class="usd">{{ $t('usd') }}</span>
        </div>
      </div>
    </div>
  </div>
</template>

<style lang="scss" scoped>
.inner {
  margin-bottom: 1rem;
  overflow: hidden;
  position: relative;
  .marquee {
    overflow: hidden;
    position: relative;
    .content {
      white-space: nowrap;
      display: inline-block;
      .crypto {
        display: inline-flex;
        font-size: 2rem;
        font-style: italic;
        padding: 0.5rem;
        margin: 0.5rem 1rem;
        //border: 1px solid lightgrey;
        //border-radius: 5px;
        //background-color: #f1f1f1;
        .name {
          margin-right: 5px;
          color: black;
          //text-shadow: 4px 4px 8px dodgerblue;
        }
        .eql {
          color: goldenrod;
          font-weight: bold;
          //text-shadow: 2px 2px 4px goldenrod;
        }
        .price {
          margin-right: 5px;
          margin-left: 5px;
          color: black;
          font-weight: bold;
          text-shadow: 1px 1px 2px dodgerblue;
        }
        .usd {
          color: black;
          margin-left: 3px;
          //text-shadow: 2px 2px 4px green;
        }
      }
    }
  }
}

@media(max-width: 1020px) {
  .inner {
    .marquee {
      .content {
        .crypto {
          font-size: 1.6rem;
          padding: 0.4rem;
        }
      }
    }
  }
}
@media (max-width: 768px) {
  .inner {
    margin-bottom: 0.5rem;
    .marquee {
      .content {
        .crypto {
          font-size: 1.5rem;
          padding: 0.3rem;
        }
      }
    }
  }
}
</style>