<script>
export default {
  props: ["value", "source", "placeholder"],
  data() {
    return {
      search: "",
      disableSearch: false,
      options: [],
      selected: {
        id: 0,
        value: ""
      },
      childs: null,
      index: 0
    };
  },
  mounted() {
    this.disableSearch = true;
    if (typeof this.value === "object") {
      this.search = this.value.value;
      this.selected = this.value;
    } else {
      this.search = this.value;
    }
    setTimeout(() => {
      this.disableSearch = false;
    }, 300);
  },
  methods: {
    listeners() {
      this.childs = this.$refs.list.querySelectorAll("li");
      document.onkeydown = e => {
        switch (e.keyCode) {
          case 38:
            if (this.index > 0) {
              this.index--;
            }
            this.childs[this.index].focus();
            break;
          case 40:
            if (this.index < this.childs.length - 1) {
              this.index++;
            }
            this.childs[this.index].focus();
            break;
          case 13:
            this.childs[this.index].click();
            break;
        }
      };
    },
    loadOptions() {
      this.axios
        .get(this.$props.source, {
          params: {
            search: this.search.toLowerCase()
          }
        })
        .then(response => {
          this.options = response.data;
        })
        .then(this.listeners);
    },
    select(e, option) {
      e.preventDefault();
      this.disableSearch = true;
      this.options = [];
      this.selected = option;
      this.search = option.value;
      this.$emit("input", this.selected);

      setTimeout(() => {
        this.disableSearch = false;
      }, 300);
    }
  },
  watch: {
    search(value) {
      if (!this.disableSearch) {
        if (value !== "") {
          this.loadOptions();
        } else {
          this.options.length = 0;
        }
      }
    },
    value(value) {
      if (typeof value === "object") {
        this.search = value.value;
      } else {
        this.search = value;
      }
    }
  }
};
</script>
<template lang="pug">
  .component
    input.form-control.text_input(type="text" v-model="search" :placeholder="placeholder" ref="result")
    ul.mdb-autocomplete-wrap(v-show="options.length > 0" ref="list")
      li(v-for="(option, index) in options" :key="option.id" @click="select($event, option)" :tabindex="index + 1") {{ option.value }}
</template>
<style lang="scss">
.mdb-autocomplete-wrap {
  max-height: 300px;
}
ul li:focus {
  background-color: darkgrey;
}
</style>
