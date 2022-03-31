<template>
  <v-card>
    <v-card-text>
      <nft-list></nft-list>
    </v-card-text>
  </v-card>
</template>

<script>
import NFTList from '@/components/NFT/NFTList.vue'
import webWallet from '@/libs/web-wallet'
import track from '@/libs/track'

export default {
  data() {
    return {
      infoLabel: [
        { label: 'address', name: 'address', copy: true },
        { label: 'balance', name: 'balance' },
        { label: 'unconfirmed_balance', name: 'unconfirmedBalance' }
      ],
      wallet: webWallet.getWallet(),
      showPriv: false
    }
  },
  components: {
    'nft-list': NFTList
  },
  props: ['view'],
  watch: {
    view: function () {
      this.wallet.setInfo()
    }
  },
  computed: {
    info: function () {
      return this.wallet.info
    },
    privKey: function () {
      return this.wallet.getPrivKey()
    }
  },
  mounted() {
    this.wallet.update()
  },
  methods: {
    onCopySucc: function () {
      track.trackAction('copy', 'view', 'privkey')
      this.$root.success('copy success')
    },
    onCopyError: function () {
      this.$root.error('copy fail')
    }
  }
}
</script>
<style lang="less" scoped>
.view-private {
  position: relative;
  &-line {
    display: flex;
    align-items: center;
    .theme--dark.v-icon {
      color: hsla(0,0%,100%,.7);
    }

    &__wrapper {
      position: absolute;
      right: 16%;
      top: 50%;
      transform: translateY(-50%);
    }
  }

}
</style>
