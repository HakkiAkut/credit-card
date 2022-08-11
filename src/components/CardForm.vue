<template>
  <div class="card column">
    <h1 class="title">Add Credit Card</h1>
    <div class="card-input">
      <div
        class="card-input__item column"
        :class="status($v.cardNumber, 'card-input__item--error')"
      >
        <label for="card-number">Card Number</label>
        <input
          v-model="$v.cardNumber.$model"
          id="card-number"
          type="text"
          placeholder="0000 0000 0000 0000"
          v-mask="'#### #### #### ####'"
        />
      </div>
      <div
        class="card-input__item column"
        :class="status($v.holderName, 'card-input__item--error')"
      >
        <label for="holder-name">Cardholder Name</label>
        <input
          v-model="$v.holderName.$model"
          id="holder-name"
          type="text"
          placeholder="Your Name"
        />
      </div>

      <div
        class="card-input__item column"
        :class="status($v.validThru, 'card-input__item--error')"
      >
        <label for="valid-thru">Valid Thru</label>
        <input
          v-model="$v.validThru.$model"
          id="valid-thru"
          type="text"
          placeholder="MM/YY"
          v-mask="'##/##'"
        />
      </div>
      <div
        class="card-input__item column"
        :class="status($v.ccv, 'card-input__item--error')"
      >
        <label for="ccv">CCV</label>
        <input
          v-model="$v.ccv.$model"
          id="ccv"
          type="text"
          placeholder="000"
          v-mask="'###'"
        />
      </div>
    </div>
    <button class="btn" @click="submitForm">Save Card</button>
  </div>
</template>

<script>
import { required, minLength } from "vuelidate/lib/validators";
export default {
  name: "CardForm",
  data: () => ({
    cardNumber: "",
    holderName: "",
    validThru: "",
    ccv: "",
  }),
  validations: {
    cardNumber: {
      required,
      minLength: minLength(19),
    },
    holderName: {
      required,
    },
    validThru: {
      required,
      minLength: minLength(5),
    },
    ccv: {
      required,
      minLength: minLength(3),
    },
  },
  methods: {
    submitForm: function () {
      this.$v.$touch();
      if (this.$v.$invalid) {
        return;
      }
      alert("SUCCESS!!");
    },
    status(validation, error) {
      return validation.$error ? error : "";
    },
  },
};
</script>

<style lang="scss" scoped>
.card {
  width: 600px;
  background-color: $white;
  border-radius: 20px;
  padding: 40px;
  flex-wrap: wrap;
  gap: 30px;
  & .title {
    @include font($quicksand, $blue-dark, 32px, 600);
  }
  &-input {
    display: grid;
    grid-template-columns: 1fr 1fr;
    grid-gap: 15px;
    &__item {
      gap: 5px;
      &:nth-of-type(2),
      &:nth-of-type(1) {
        grid-column: span 2;
      }
      & input[type="text"] {
        width: 100%;
        padding: 12px;
        border: 1px solid $blue-light;
        border-radius: 10px;
        @include font($inter, $blue-dark, 18px, 400);
        line-height: 24px;
        &:focus {
          outline: none !important;
          border: 2px solid $blue !important;
        }
      }
      & label {
        @include font($inter, $blue, 14px, 300);
        padding: 0 4px;
        margin-bottom: -12px;
        background-color: $white;
        width: fit-content;
        z-index: 2;
      }
      @at-root #{&}--error {
        & input[type="text"] {
          border: 1px solid red !important;
        }
      }
    }
  }
  & .btn {
    height: 50px;
    border: 0;
    border-radius: 10px;
    cursor: pointer;
    @include font($quicksand, $white, 18px, 500);
    background-image: linear-gradient(to right, $blue-light, $blue);
    &:hover {
      background-image: linear-gradient(to right, $blue, $blue-dark);
    }
  }
}
</style>
