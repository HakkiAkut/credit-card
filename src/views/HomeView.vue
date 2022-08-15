<template>
  <div class="home">
    <card-form v-model="form" />
    <credit-card
      :cardNumber="form.cardNumber"
      :holderName="form.holderName"
      :validThru="form.validThru"
      :cvv="form.cvv"
      :input="form.input"
    />
  </div>
</template>

<script>
import CardForm from "@/components/CardForm.vue";
import CreditCard from "../components/CreditCard.vue";
export default {
  name: "HomeView",
  components: { CardForm, CreditCard },
  methods: {},
  data: () => ({
    form: {
      cardNumber: "",
      holderName: "",
      validThru: "",
      cvv: "",
      input: "",
    },
  }),
};
</script>

<style lang="scss">
.home {
  @include flex(row, center, center);
  gap: 25px;
}
</style>
