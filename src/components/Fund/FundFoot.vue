<template>
  <v-footer
    class="fund__footer"
    padless
    dark
    :class="colorTheme"
  >
    <v-card
      class="flex"
      flat
      tile
      style="background:transparent"
    >
      <v-card-title
        class="py-2 px-4"
        v-if="mode === 'expense'"
      >
        <div class="percent">
          {{ Math.round((monthBudget - monthExpense) / monthBudget * 100) }} %
        </div>
        <v-spacer></v-spacer>
        <div class="d-flex align-center">
          <v-icon large>attach_money</v-icon>
          <div class="text-right">
            <span class="overline d-block"> 預算餘額 </span>
            <span class="headline">
              <span>{{ numberFormat(monthBudget - monthExpense) }}</span>
            </span>
          </div>
          <div class="pl-5 pr-4 headline">
            /
          </div>
          <div class="text-right">
            <span class="overline d-block"> 當月預算 </span>
            <span class="headline">
              <span>{{ numberFormat(monthBudget) }}</span>
            </span>
          </div>
        </div>
      </v-card-title>
      <v-card-title
        class="py-2 px-4"
        v-if="mode === 'income'"
      >
        <v-spacer></v-spacer>
        <div class="d-flex align-center">
          <v-icon large>attach_money</v-icon>
          <div class="text-right">
            <span class="overline d-block"> 當月收入 </span>
            <span class="headline">
              <span>{{ numberFormat(monthIncome) }}</span>
            </span>
          </div>
        </div>
      </v-card-title>
    </v-card>
  </v-footer>
</template>

<script>
  export default {
    computed: {
      mode() {
        return this.$store.state.route.name;
      },
      colorTheme() {
        return this.$store.getters["getColorTheme"];
      },

      monthExpense() {
        return this.$store.getters["getMonthExpenseTotal"];
      },
      monthIncome() {
        return this.$store.getters["getMonthIncomeTotal"];
      },
      monthBudget() {
        return this.$store.getters["getBudget"];
      }
    },
    methods: {
      numberFormat(num) {
        return new Intl.NumberFormat("en-US").format(num);
      }
    }
  };
</script>

<style lang="scss" scoped>
  .fund__footer {
    position: fixed;
    right: 0;
    bottom: 0;
    width: 100%;
    padding-left: 256px;
    transition: width 0.3s 0.3s;
    @media screen and (max-width: 1264px) {
      width: 100%;
      transition: width 0.3s;
      padding-left: 0;
    }
    .headline {
      > * {
        vertical-align: middle;
      }
    }
  }
  .v-navigation-drawer--close {
    .fund__footer {
      width: 100%;
    }
  }
</style>