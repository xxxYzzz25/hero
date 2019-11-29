<template>
  <div class="heroBox">
    <loading :active.sync="isLoading" :is-full-page="false"></loading>
    <div
      class="heroItem"
      :class="{'clickHero':clickHero == index}"
      v-for="(item,index) in heroItem"
      :key="index"
      @click="checkHero(item,index)"
    >
      <div class="heroImg">
        <img :src="item.image" alt />
      </div>
      <div class="heroName">{{item.name}}</div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      isLoading: false,
      heroItem: [],
      tempHero: {},
      clickHero: -1
    };
  },
  methods: {
    getHeros() {
      const api = "http://hahow-recruit.herokuapp.com/heroes",
        vm = this;
      vm.isLoading = true;
      this.$axios.get(api).then(res => {
        setTimeout(() => {
          vm.isLoading = false;
        }, 1000);
        vm.heroItem = res.data;
      });
    },
    checkHero(item, index) {
      this.tempHero = Object.assign({}, item);
      const vm = this,
        api = `http://hahow-recruit.herokuapp.com/heroes/${vm.tempHero.id}/profile`;
      vm.isLoading = true;
      this.$axios.get(api).then(res => {
        setTimeout(() => {
          vm.isLoading = false;
        }, 1000);
        vm.$bus.$emit("heroProfile", res.data);
        vm.$bus.$emit("heroId", vm.tempHero.id);
      });
      vm.clickHero = index;
    }
  },
  created() {
    this.getHeros();
  }
};
</script>

<style scoped lang="scss">
.heroBox {
  width: 100%;
  display: flex;
  flex-wrap: wrap;
  justify-content: space-around;
  border: 5px solid #1d7874;
  border-radius: 3px;
  .heroItem {
    margin: 20px;
    padding: 10px;
    text-align: center;
    color: #069;
    font-weight: bold;
    font-size: 20px;
    transition: 0.2s;
    background: #22223b;
    border-radius: 3px;
    cursor: pointer;
    filter: grayscale(0.5);
    &:hover {
      filter: grayscale(0);
      box-shadow: 0px 0px 16px #c5d1eb, 0 0 10px #fff;
    }
    .heroImg {
      margin-bottom: 10px;
      img {
        width: 100%;
      }
    }
  }
  .clickHero {
    filter: grayscale(0);
    box-shadow: 0px 0px 16px #c5d1eb, 0 0 10px #fff;
  }
}
@media screen and (max-width: 1024px) and (min-width: 768px) {
  .heroBox {
    .heroItem {
      width: 35%;
      font-size: 32px;
    }
  }
}
@media screen and (max-width: 767px) {
  .heroBox {
    .heroItem {
      width: 75%;
      font-size: 48px;
    }
  }
}
</style>
