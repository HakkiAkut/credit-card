<template>
  <div class="card" :class="{ rotate: !rotate }" @click="changeSide">
    <div class="card__side column card__side--front" v-if="front">
      <h2 class="card-title">CREDIT CARD</h2>
      <svg
        class="icon-chip"
        viewBox="0 0 48 48"
        enable-background="new 0 0 48 48"
      >
        <path
          fill="#FF9800"
          d="M5,35V13c0-2.2,1.8-4,4-4h30c2.2,0,4,1.8,4,4v22c0,2.2-1.8,4-4,4H9C6.8,39,5,37.2,5,35z"
        />
        <g fill="#FFD54F">
          <path
            d="M43,21v-2H31c-1.1,0-2-0.9-2-2s0.9-2,2-2h1v-2h-1c-2.2,0-4,1.8-4,4s1.8,4,4,4h3v6h-3c-2.8,0-5,2.2-5,5 s2.2,5,5,5h2v-2h-2c-1.7,0-3-1.3-3-3s1.3-3,3-3h12v-2h-7v-6H43z"
          />
          <path
            d="M17,27h-3v-6h3c2.2,0,4-1.8,4-4s-1.8-4-4-4h-3v2h3c1.1,0,2,0.9,2,2s-0.9,2-2,2H5v2h7v6H5v2h12 c1.7,0,3,1.3,3,3s-1.3,3-3,3h-2v2h2c2.8,0,5-2.2,5-5S19.8,27,17,27z"
          />
        </g>
      </svg>

      <transition-group name="list" tag="p">
        <span
          v-for="item in card.cardNumber"
          v-bind:key="item"
          class="card-num"
        >
          {{ item == " " ? "&nbsp;" : item }}
        </span>
      </transition-group>

      <div class="card-valid column">
        <p class="card-valid__text">Valid Thru</p>
        <transition-group name="list" tag="p">
          <span
            v-for="item in card.validThru"
            v-bind:key="item"
            class="card-valid__text card-valid__text--num"
          >
            {{ item }}
          </span>
        </transition-group>
      </div>
      <div class="cardholder-name">{{ holderName }}</div>
    </div>
    <div class="card__side column card__side--back" v-else>
      <div class="cvv-line">
        <transition-group name="list" tag="p">
          <span
            v-for="item in card.cvv"
            v-bind:key="item"
            class="cvv-line__text"
          >
            {{ item }}
          </span>
        </transition-group>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "CreditCard",
  data: () => ({
    rotate: true,
    front: true,
    card: {
      cardNumber: [],
      validThru: [],
      holderName: "",
      cvv: [],
    },
  }),
  props: {
    cardNumber: String,
    validThru: String,
    holderName: String,
    cvv: String,
    input: String,
  },
  mounted() {
    this.splitNum();
    this.splitCvv();
    this.splitValidThru();
  },
  watch: {
    cardNumber() {
      this.splitNum();
    },
    cvv() {
      this.splitCvv();
    },
    validThru() {
      this.splitValidThru();
    },
    input() {
      if (this.input != "cvv") {
        this.changeDirection(true);
      } else {
        this.changeDirection(false);
      }
    },
  },
  methods: {
    changeSide: function () {
      this.initial = false;
      this.rotate = !this.rotate;
      setTimeout(() => (this.front = this.rotate), 250);
    },
    changeDirection: function (direction) {
      this.initial = false;
      this.rotate = direction;
      setTimeout(() => (this.front = this.rotate), 250);
    },
    splitNum: function () {
      let list = this.cardNumber.split("");
      if (list.length < 19) {
        for (var i = list.length; i < 19; i++) {
          var el = (i + 1) % 5 == 0 ? " " : "*";
          list.push(el);
        }
      }
      this.card.cardNumber = list;
    },
    splitCvv: function () {
      let list = this.cvv.split("");
      if (list.length < 3) {
        for (var i = list.length; i < 3; i++) {
          list.push("*");
        }
      }
      this.card.cvv = list;
    },
    splitValidThru: function () {
      let list = this.validThru.split("");
      if (list.length < 5) {
        for (var i = list.length; i < 5; i++) {
          var el = (i + 1) % 3 == 0 ? "/" : "*";
          list.push(el);
        }
      }
      this.card.validThru = list;
    },
  },
};
</script>

<style lang="scss" scoped>
$animation-duration: 1s;
$width: 580px;
$height: 360px;
.card {
  position: relative;
  width: $width;
  height: $height;
  border-radius: 20px;
  transform-style: preserve-3d;
  animation-fill-mode: forwards;
  transition: transform 1s;
  &__side {
    padding: 30px 40px;
    border-radius: inherit;
    width: 100%;
    height: 100%;
    position: absolute;
    animation-fill-mode: forwards;
    backface-visibility: hidden;
    background-image: linear-gradient($blue, $blue-light);
    gap: 20px;

    .card-num {
      @include font($credit, $white, 24px);
      word-spacing: 5px;
      letter-spacing: 3px;
      display: inline-block;
    }
    .card-valid {
      &__text {
        @include font($inter, $grey, 14px);
        @at-root #{&}--num {
          color: $white;
        }
      }
    }
    .cardholder-name {
      @include font($credit, $white, 24px);
      word-spacing: 3px;
      letter-spacing: 1px;
    }

    @at-root #{&}--front {
      & .card-title {
        @include font($inter, $white, 28px, 500);
        text-align: right;
      }
      & .icon-chip {
        width: 64px;
        height: 64px;
      }
    }
    @at-root #{&}--back {
      position: relative;
      justify-content: center;
      transform: rotateY(180deg);
      &:before {
        content: "";
        position: absolute;
        background-color: $black;
        height: 80px;
        width: 100%;
        top: 30px;
        left: 0;
      }
      .cvv-line {
        position: relative;
        width: 80%;
        height: 50px;
        background-color: $white;
        padding: 10px;
        display: flex;
        align-items: center;
        justify-content: right;
        &__text {
          @include font($inter, $black, 18px);
        }
      }
    }
  }
  &.rotate {
    transform: rotateY(180deg);
    transition: transform 0.5s;
  }
}
.list-enter-active {
  transition: all 1s;
}
.list-enter {
  opacity: 0;
  transform: translateY(30px);
}
</style>
