<script>
export default {
  props: ["value", "options", "placeholder", "searchable"],
  data() {
    return {
      component_id: null,
      list: false,
      filter: "",
      selection: {},
      filterOptions: [],
      childs: null,
      index: 0
    };
  },
  created() {
    this.component_id = "_" + this._uid;
    let self = this;
    window.addEventListener("click", function(e) {
      // close dropdown when clicked outside
      if (!self.$el.contains(e.target)) {
        self.list = false;
      }
    });
    this.filterOptions = this.options;
    this.selection = this.value;
  },
  methods: {
    listeners() {
      this.childs = this.$refs.component.querySelectorAll("li:not(.optgroup)");
    
      this.$refs.component.onkeydown = e => {
         if([38,40,13].includes(e.keyCode)){
          e.preventDefault()
        }
        this.childs[this.index].className = "";
        switch (e.keyCode) {
          case 38:
            if (this.index > 0) {
              this.index--;
            }      
            break;
          case 40:
            if (this.index < this.childs.length - 1) {
              this.index++;
            }     
            break;
          case 13:
            this.childs[this.index].click();
            break;
        }
        this.childs[this.index].focus();
        this.childs[this.index].className = "active";
      };
    },
    listOptions() {
      this.list = !this.list;
      this.listeners();
    },
    select(option) {
      this.selection = option;
      this.list = false;
      this.$emit("input", option);
    }
  },
  computed: {
    placeholder_text() {
      if (this.selection && this.selection.label) {
        return this.selection.label;
      }
      return this.placeholder;
    }
  },
  watch: {
    value(value) {
      this.selection = value;
    },
    filter(value) {
      this.filterOptions = this.$props.options.filter(option => {
        if (option.options) {
          return (
            option.options.filter(subopt => {
              return (
                subopt.label.toLowerCase().indexOf(value.toLowerCase()) > -1
              );
            }).length > 0
          );
        }
        return (
          option.label.toLowerCase().indexOf(value.toLowerCase()) > -1 ||
          value == ""
        );
      });
    },
    options(value) {
      this.filterOptions = value;
    }
  }
};
</script>
<template lang="pug">
  .select-wrapper.mdb-select.md-form.job-search-control(ref="component")
    span.caret ▼
    input.select-dropdown(type="text" readonly="true" :placeholder="placeholder_text" @click="listOptions")
    ul.dropdown-content.select-dropdown.w-100.multiple-select-dropdown(v-show="list" :id="component_id" ref="list")
      span.search-wrap.ml-2(v-if="searchable")
        .md-form.mt-0
          input.search.form-control.w-100.d-block(type="text" placeholder="Search here.." v-model="filter")
      div(v-for="option in filterOptions" :key="option.id")
        li(class="optgroup" v-if="option.options")
          span {{ option.label }}
        li(v-if="option.options" v-for="subOption in option.options" @click="select(subOption)")
          span.filtrable
            | {{ subOption.label }}
        li(@click="select(option)" v-if="!option.options")
          span.filtrable
            | {{ option.label }}
</template>
<style lang="scss" scoped>
ul li.active {
  background-color: darkgray;
}
.dropdown-content.select-dropdown.w-100.multiple-select-dropdown {
  display: block;
  opacity: 1;
  max-height: 300px;
  position: absolute;
}
.select-dropdown {
  width: calc(100% - 13px) !important;
}
</style>
