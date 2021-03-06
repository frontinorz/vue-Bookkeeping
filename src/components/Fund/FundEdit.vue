<template>
  <v-card>
    <v-card-title :class="colorTheme">
      <span class="white--text">{{ inputMode.title }}</span>
    </v-card-title>
    <v-container>
      <v-col cols="12">
        <v-chip-group
          mandatory
          active-class="blue accent-4 white--text"
          column
          v-model=category_id
        >
          <v-chip
            class="mr-2"
            v-for="item in category"
            :key="item.id"
            @click="categoryHandler(item.id)"
          >
            <v-icon>{{ item.icon }}</v-icon>
            <span>{{ item.title }}</span>
          </v-chip>
        </v-chip-group>
      </v-col>
      <v-col cols="12">
        <v-text-field
          type="number"
          prepend-icon="mdi-currency-usd"
          placeholder="0"
          v-model.number="amount"
          :rules="amountRule"
          validate-on-blur="true"
        />
      </v-col>
      <v-col cols="12">
        <v-textarea
          :placeholder="inputMode.placeholder"
          rows="1"
          prepend-icon="comment"
          v-model="descr"
        ></v-textarea>
      </v-col>
      <v-card-actions>
        <v-spacer></v-spacer>
        <v-btn
          text
          color="primary"
          @click="editHandler"
        >
          確認
        </v-btn>
        <v-btn
          text
          color="error"
          @click="closeHandler"
        >
          取消
        </v-btn>
      </v-card-actions>
    </v-container>
  </v-card>
</template>

<script>
  export default {
    props: {
      target: {
        type: Object,
        required: true
      }
    },
    data() {
      return {
        modal: false,
        amount: this.target.amount,
        descr: this.target.descr,
        category_id: this.target.category_id,
        modeList: [
          {
            mode: "expense",
            title: "修改支出",
            placeholder: "消費描述..."
          },
          {
            mode: "income",
            title: "修改收入",
            placeholder: "收入描述..."
          }
        ],
        amountRule: [value => value > 0 || "此為必須欄位"]
      };
    },
    computed: {
      mode() {
        return this.$store.state.route.name;
      },
      category() {
        return this.$store.getters["getCategoryList"](this.mode);
      },
      inputMode() {
        return this.modeList.find(
          item => item.mode === this.$store.state.route.name
        );
      },
      colorTheme() {
        return this.$store.getters["getColorTheme"];
      }
    },
    methods: {
      categoryHandler(id) {
        this.category_id = id;
      },
      async editHandler() {
        if (!this.amount) return;

        let obj = {
          id: this.target.id,
          amount: this.amount,
          descr: this.descr,
          category_id: this.category_id
        };
        if (this.mode === "expense") {
          await this.$store.dispatch("UPDATE_EXPENSE", obj);
        }
        if (this.mode === "income") {
          await this.$store.dispatch("UPDATE_INCOME", obj);
        }
        this.$emit("closeHandler");
      },
      closeHandler() {
        this.$emit("closeHandler");
      }
    },
    watch: {
      target: function(newVal) {
        // watch it
        this.amount = newVal.amount;
        this.descr = newVal.descr;
        this.category_id = newVal.category_id;
      }
    }
  };
</script>

<style>
</style>