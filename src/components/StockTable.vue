<template>
  <section>
    <base-button @click.native="loadData">Get Data</base-button>
    <no-data v-if="error"></no-data>
    <loader v-if="isLoading && !isLoaded"></loader>
    <div v-if="isLoaded" class="table table--3cols">
      <div class="table__row">
        <div class="table_cell table__cell--darkgrey"><h3>Stock</h3></div>
        <div
          class="table__cell"
          v-for="stock in stocks"
          :key="stocks.indexOf(stock)"
        >
          {{ stock.name }}
        </div>
      </div>

      <div class="table__row">
        <div class="table_cell table__cell--darkgrey"><h3>Current</h3></div>
        <div
          class="table__cell"
          v-for="stock in stocks"
          :key="stocks.indexOf(stock)"
        >
          {{ stock.current }}
        </div>
      </div>

      <div class="table__row">
        <div class="table_cell table__cell--darkgrey"><h3>Change</h3></div>
        <div
          class="table__cell"
          v-for="stock in stocks"
          :key="stocks.indexOf(stock)"
          :class="[stock.change < 0 ? 'negative' : 'positive']"
        >
          {{ stock.change }}
        </div>
      </div>
    </div>
  </section>
</template>

<script>
import BaseButton from "./BaseButton.vue";
import NoData from "./messages/NoData";
import Loader from "./messages/Loader";

import { simulateAsyncReq } from "../plugins/getDataFunc";
import { payload } from "../mocData/index";

export default {
  components: { BaseButton, NoData, Loader },
  data() {
    return {
      isLoaded: false,
      isLoading: false,
      error: false,

      obj: {},
      stocks: [],
    };
  },
  methods: {
    reduceNumber(num) {
      // rounds the number to the second decimal place
      const res = Math.round((num + Number.EPSILON) * 100) / 100;
      return res.toFixed(2);
    },
    countChangeValue(current, start) {
      const change = this.reduceNumber(current - start);
      if (change < 0) {
        return change.toString();
      } else {
        return `+${change}`;
      }
    },
    getNewValues() {
      for (let i = 0; i < this.obj.stocks.length; i++) {
        this.stocks.push({
          name: this.obj.stocks[i],
          start: this.reduceNumber(this.obj.start[i]),
          current: this.reduceNumber(this.obj.current[i]),
          change: this.countChangeValue(this.obj.current[i], this.obj.start[i]),
        });
      }
      this.stocks.sort((a, b) => (a.name >= b.name ? 1 : -1));
    },
    loadData() {
      this.stocks = [];
      this.isLoaded = false;
      this.error = false;
      this.isLoading = true;

      let result = simulateAsyncReq(payload);
      result.then(
        (response) => {
          this.obj = response;
          this.isLoaded = true;
          this.isLoading = false;

          this.getNewValues();
        },
        () => {
          this.isLoading = false;
          this.error = true;
        }
      );
    },
  },
};
</script>

<style lang="scss" scoped>
.table {
  display: flex;
  flex-wrap: wrap;
  margin: 0 10em 3em 10em;
  padding: 0;
  border-radius: 2px;
  border: solid 1px #ededed;

  .negative {
    color: crimson;
  }

  .positive {
    color: #a4ca7f;
  }

  &__row {
    display: flex;
    flex-direction: column;
  }

  &__cell {
    box-sizing: border-box;
    flex-grow: 1;
    width: 100%; // Default to full width
    padding: 0.8em 1.2em;
    overflow: hidden; // Or flex might break
    list-style: none;
    border-top: solid 1px #ededed;
    background-color: #fff;
    font-weight: 450;

    &--darkgrey {
      text-align: center;
      background-color: #f5f5f5;
    }
  }
}

.table--3cols {
  .table__row {
    width: 33.33%;
  }
}

h3 {
  margin: 0;
  padding: 0.8em 1.2em;
  font-weight: 900;
}
</style>
