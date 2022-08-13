<template>
  <div class="home">
    <card-form />
    <credit-card />
  </div>
</template>

<script>
import CardForm from "@/components/CardForm.vue";
import CreditCard from "../components/CreditCard.vue";
export default {
  name: "HomeView",
  components: { CardForm, CreditCard },
  methods: {},
};
</script>

<style lang="scss">
.home {
  @include flex(row, center, center);
  gap: 25px;
}
</style>
