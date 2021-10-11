<template>
  <div class="custom-select">
    <div
      class="selected"
      tabindex="0"
      :class="{ open: open }"
      @click="toggleDropdown"
      @keyup.enter="toggleDropdown"
    >
      <img :src="currency.flag" />
      <span>{{ currency.code }}</span>
    </div>

    <div class="dropdown" :class="{ selectHide: !open }">
      <input
        type="text"
        tabindex="0"
        class="search"
        v-model="search"
        placeholder="search..."
      />

      <div class="items">
        <div
          tabindex="0"
          class="option"
          v-for="currency in filteredCurrencies"
          :key="currency.code"
          @keyup.enter="
            selected = currency;
            open = false;
            $emit('currency-selected', currency);
          "
          @click="
            selected = currency;
            open = false;
            $emit('currency-selected', currency);
          "
        >
          <img :src="currency.flag" />
          <span>{{ currency.code }}</span>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    currencies: {
      type: Array,
      required: true,
    },
    currency: {
      type: Object,
      required: true,
    },
  },
  data() {
    return {
      open: false,
      search: "",
    };
  },
  computed: {
    filteredCurrencies: function () {
      var self = this;
      return this.currencies.filter((currency) => {
        return currency.search.indexOf(self.search.toUpperCase()) >= 0;
      });
    },
  },
  methods: {
    toggleDropdown() {
      this.open = !this.open;
      this.search = "";
    },
    close(e) {
      if (!this.$el.contains(e.target)) {
        this.open = false;
      }
    },
  },
  mounted() {
    document.addEventListener("click", this.close);
  },
  beforeUnmount() {
    document.removeEventListener("click", this.close);
  },
};
</script>

<style scoped>
.selected,
.option {
  display: flex;
  flex-direction: row;
}

img {
  margin: 20px 0;
  height: 20px;
  border: 1px #ccc solid;
}

span {
  padding: 20px 0 20px 7px;
}

.search {
  width: 88px;
  height: 60px;
}

.dropdown {
  margin-top: -67px;
}

.custom-select {
  position: relative;
  width: 120px;
  outline: none;
}

.custom-select .selected {
  padding: 0 10px;
  height: 60px;
  margin: 5px 5px 0 5px;
  font-size: 18px;
  border: 1px #ccc solid;
  border-radius: 3px;
  color: #263a66;
  outline-color: #00e7eb;
  cursor: pointer;
  user-select: none;
}

.custom-select .selected.open {
  border: 1px solid #00e7eb;
}

.custom-select .selected:after {
  position: absolute;
  content: "";
  top: 32px;
  right: 1em;
  width: 0;
  height: 0;
  border: 5px solid transparent;
  border-color: #263a66 transparent transparent transparent;
}

.custom-select .items {
  border-radius: 3px;
  overflow: scroll;
  height: 400px;
  border: 1px solid #999;
  position: absolute;
  background-color: white;
  left: 5px;
  right: 5px;
  z-index: 1;
}

.custom-select .items div {
  color: #263a66;
  padding-left: 1em;
  cursor: pointer;
  user-select: none;
}

.custom-select .items div:hover,
.custom-select .items div:focus {
  background-color: #00c7cb;
  color: white;
  outline: none;
}

.selected:focus {
  border: 1px solid #00c7cb;
}

.selected:hover {
  border: 1px black solid;
}

.selectHide {
  visibility: hidden;
}
</style>
