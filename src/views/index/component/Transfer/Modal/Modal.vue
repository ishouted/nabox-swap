<template>
  <div class="mask-cont" :class="{'show_modal': showModal}">
    <div class="modal-cont" :class="{'show_modal-cont': showModal}">
      <div class="header-cont size-36 font-500 mt-2">
          {{ $t('modal.modal1') }}
        <div class="back-icon" @click="back">
          <svg t="1626400145141" class="icon" viewBox="0 0 1127 1024" version="1.1" xmlns="http://www.w3.org/2000/svg" p-id="1446" width="17" height="15"><path d="M1058.133333 443.733333H233.130667l326.997333-327.338666a68.266667 68.266667 0 0 0 0-96.256 68.266667 68.266667 0 0 0-96.256 0l-443.733333 443.733333a68.266667 68.266667 0 0 0 0 96.256l443.733333 443.733333a68.266667 68.266667 0 0 0 96.256-96.256L233.130667 580.266667H1058.133333a68.266667 68.266667 0 1 0 0-136.533334z" fill="#333333" p-id="1447"></path></svg>
        </div>
      </div>
      <div class="search-cont">
        <span class="search-icon">
          <img src="@/assets/image/search.png" alt="">
        </span>
        <input type="text" v-model="searchVal" :placeholder="$t('modal.modal2')" >
      </div>
      <div class="search-result">
        <div class="coin-list" v-if="showList.length > 0">
          <div class="list-item cursor-pointer" v-for="(item, index) in showList" :key="index">
            <div class="d-flex align-items-center space-between pr-4 flex-1" @click="selectAsset(item)">
              <div class="coin-item">
                <span class="coin-icon">
                  <img :src="item.icon || getPicture(item.symbol) || pictureError" @error="pictureError">
                </span>
                <span class="d-flex direction-column h-40" :class="type==='assets' && 'space-between' || 'justify-content-center'">
                  <span class="text-3a font-500">{{ item.symbol }}</span>
                  <span class="sign size-16" v-if="type==='assets'">{{ item.registerChain }}</span>
                </span>
              </div>
              <span class="text-3a font-500 size-30">{{ (item.userBalance || 0) | numberFormat }}</span>
            </div>
          </div>
        </div>
        <div class="text-center text-90 flex-1 pt-4" v-else>{{ $t('modal.modal3') }}</div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Modal",
  props: {
    assetList: {
      type: Array,
      default: () => []
    },
    showModal: {
      type: Boolean,
      default: false
    },
    type: {
      type: String,
      default: 'assets'
    }
  },
  watch: {
    assetList: {
      immediate: true,
      deep: true,
      handler(val) {
        if (val) {
          this.allList = val;
          if (this.searchVal) {
            this.showList = this.allList.filter(v => {
              const search  = val.toUpperCase();
              const symbol = v.symbol.toUpperCase();
              return symbol.indexOf(search) > -1;
            })
          } else {
            this.showList = val;
          }
        }
      }
    },
    searchVal(val) {
      if (val) {
        this.showList = this.allList.filter(v => {
          const search  = val.toUpperCase();
          const symbol = v.symbol.toUpperCase();
          return symbol.indexOf(search) > -1;
        });
      } else {
        this.showList = this.allList;
      }
    },
  },
  data() {
    return {
      searchVal: '',
      allList: [],
      showList: []
    }
  },
  methods: {
    back() {
      this.searchVal = '';
      this.$emit('update:showModal', false)
    },
    selectAsset(asset) {
      this.searchVal = '';
      this.$emit('selectAsset', asset)
    }
  }
}
</script>

<style scoped lang="scss">
@import "Modal";
</style>