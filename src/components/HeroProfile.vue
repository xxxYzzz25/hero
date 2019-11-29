<template>
  <div class="profileBox" v-show="isShow">
    <div class="left">
      <div v-for="(item, index) in profiles" :key="index" class="profileItem">
        <div class="pName">{{ index.toUpperCase() }}:</div>
        <div class="pValue">
          <a :class="{'isClick':item !== 0}" @click="decrease(index)">-</a>
          {{ item }}
          <a :class="{'isClick':click}" @click="increase(index)">+</a>
        </div>
      </div>
    </div>
    <div class="right">
      <div class="total">POINTS : {{ left }}</div>
      <a :class="{'isSave':save}" @click="send">
        <loading :active.sync="isLoading" :is-full-page="false"></loading>
        {{saveText}}
      </a>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      isLoading: false,
      profiles: {},
      heroId: "",
      left: 0,
      sum: 0,
      isShow: false,
      isClick: false,
      isSave: true,
      saveText: "SAVE"
    };
  },
  methods: {
    increase(i) {
      if (this.left > 0) {
        this.profiles[i]++;
        this.left--;
      }
    },
    decrease(i) {
      if (this.profiles[i] > 0) {
        this.profiles[i]--;
        this.left++;
      } else {
        return;
      }
    },
    send() {
      const vm = this,
        api = `http://hahow-recruit.herokuapp.com/heroes/${vm.heroId}/profile`;
      vm.isLoading = true;
      vm.saveText = "SAVING...";
      this.$axios.patch(api, vm.profiles).then(res => {
        setTimeout(() => {
          vm.isLoading = false;
          vm.saveText = "SAVE";
        }, 1000);
        console.log(res.data);
      });
    }
  },
  computed: {
    click() {
      return this.left == 0 ? (this.isClick = false) : (this.isClick = true);
    },
    save() {
      return this.left == 0 ? (this.isSave = true) : (this.isSave = false);
    }
  },
  created() {
    const vm = this;
    vm.$bus.$on("heroProfile", res => {
      vm.profiles = res;
      setTimeout(() => {
        vm.isShow = true;
      }, 1000);
    });
    vm.$bus.$on("heroId", res => {
      vm.heroId = res;
    });
  }
};
</script>

<style scoped lang="scss">
a {
  display: block;
  color: #8895b3;
  background: #ccc;
  border: 5px solid #999;
  outline: none;
  display: flex;
  justify-content: center;
  align-items: center;
}
.profileBox {
  position: relative;
  margin-top: 5%;
  border: 5px solid #1d7874;
  display: flex;
  justify-content: space-evenly;
  align-items: flex-end;
  color: #09f;
  font-weight: bold;
  .left {
    width: 45%;
    font-size: 20px;
    .profileItem {
      display: flex;
      align-items: center;
      margin: 3% 2%;
      .pName {
        width: 15%;
      }
      .pValue {
        width: 25%;
        display: flex;
        justify-content: space-between;
        align-items: center;
        a {
          width: 40px;
          height: 40px;
          font-size: 28px;
          cursor: not-allowed;
        }
        .isClick {
          color: #1d7874;
          border: 5px solid #1d7874;
          background: #c5d1eb;
          cursor: pointer;
        }
      }
    }
  }
  .right {
    width: 40%;
    display: flex;
    justify-content: space-around;
    align-items: center;
    margin-bottom: 1%;
    font-size: 28px;
    a {
      width: 35%;
      padding: 5%;
      font-size: 28px;
      cursor: not-allowed;
      position: relative;
    }
    .isSave {
      color: #1d7874;
      border: 5px solid #1d7874;
      background: #c5d1eb;
      cursor: pointer;
    }
  }
}
@media screen and (max-width: 1024px) and (min-width: 768px) {
  .profileBox {
    .left {
      .profileItem {
        .pValue {
          width: 35%;
        }
      }
    }
  }
}
@media screen and (max-width: 767px) {
  .profileBox {
    flex-direction: column;
    align-items: center;
    font-size: 40px;
    .left {
      width: 100%;
      font-size: 46px;
      margin-bottom: 5%;
      .profileItem {
        justify-content: center;
        .pName {
          width: 25%;
        }
        .pValue {
          width: 35%;
          a {
            width: 60px;
            height: 60px;
            font-size: 40px;
          }
        }
      }
    }
    .right {
      width: 100%;
      flex-direction: column;
      margin-bottom: 2%;
      font-size: 46px;
      a {
        margin-top: 3%;
        width: 65%;
        padding: 3%;
        font-size: 40px;
      }
    }
  }
}
</style>
