<template>
  <div class="row">
    <div class="col">
      <Card :title="'Vexchange ' + $t('price')">
        <div class="wrapper">
          <div class="item">
            <div class="title">
              vet {{ $t('price') }}
            </div>
            <div class="description">
              {{ format(contractPrice.vet) }}
            </div>
          </div>
          <div class="item">
            <div class="title">
              vtho {{ $t('price') }}
            </div>
            <div class="description">
              {{ format(contractPrice.vtho, 10, false) }}
            </div>
          </div>
        </div>
      </Card>
    </div>
    <div class="col">
      <Card :title="'Coinmarketcap ' + $t('price')">
        <div class="wrapper">
          <div class="item">
            <div class="title">
              vet {{ $t('price') }}
            </div>
            <div class="description">
              {{ conversion(prices.vet) }}
            </div>
          </div>
          <div class="item">
            <div class="title">
              vtho {{ $t('price') }}
            </div>
            <div class="description">
              {{ conversion(prices.vtho) }}
            </div>
          </div>
        </div>
      </Card>
    </div>
  </div>
</template>

<script>
import Card from './Card';
export default {
  name: 'Prices',
  components: { 
    Card,
  },
  props: ['prices'],
  data() {
    return {
      balance: {
        vet: 0,
        vtho: 0,
      }
    }
  },
  mounted() {
    this.$getBalance().then(data => {
      this.balance = data;
    });
  },
  computed: {
    contractPrice() {
      return {
        vet: ((this.balance.vtho / this.balance.vet) * this.prices.vtho) * 100,
        vtho: ((this.balance.vet / this.balance.vtho) * this.prices.vet) * 100,
      }
    }
  },
  methods: {
    conversion(num) {
      return (num * 100).toFixed(3);
    },
    format(num, mul = true) {
      return mul ? num.toFixed(3) : (num / 10).toFixed(3);
    }
  }
}
</script>

<style lang="scss" scoped>
.wrapper {
  display: flex;
  justify-content: space-between;
}

.title {
  font-size: 0.8em;
  font-weight: 300;
  text-transform: uppercase;
}

.description {
  color: #1775ff;
  font-size: 3em;
  font-weight: 300;
  line-height: 1;

  &::after {
    content: '¢';
    font-size: 0.7em;
    margin-left: -10px;
  }

  @media all and (max-width: 768px) {
    font-size: 2em;
  }
}
</style>
