<script>
export default {
  props: ["value", "options", "placeholder", "depend"],
  data() {
    return {
      component_id: null,
      list: false,
      filter: "",
      selections: [],
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
    this.selections = this.value;
    this.filterOptions = this.options;
  },
  methods: {
    listeners() {
      this.childs = document.querySelectorAll(
        "#" + this.component_id + " span:not(.optgroup)"
      );

      document.onkeydown = e => {
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
      if (this.$props.depend && this.$props.options.length === 0) {
        this.alert("Wait, please select a " + this.$props.depend + " first");
      } else {
        this.list = !this.list;
      }
      this.listeners();
    },
    select(id) {
      if (this.selections.indexOf(id) == -1) {
        this.selections.push(id);
      } else {
        this.selections = this.selections.filter(selected => {
          return selected != id;
        });
      }
      this.$emit("input", this.selections);
    },
    GUID(index) {
      return (
        this.$props.placeholder.replace(/\s/g, "").toLowerCase() + "_" + index
      );
    }
  },
  watch: {
    filter(value) {
      let options = JSON.parse(JSON.stringify(this.options));
      this.filterOptions = options.filter(option => {
        if (option.options) {
          option.options = option.options.filter(subopt => {
            return subopt.label.toLowerCase().indexOf(value.toLowerCase()) > -1;
          });
          return option.options.length > 0;
        }
        return (
          option.label.toLowerCase().indexOf(value.toLowerCase()) > -1 ||
          value === ""
        );
      });
    },
    options(value) {
      this.filterOptions = value;
    },
    value(value) {
      this.selections = value;
    },
    selections() {
      this.$emit("input", this.selections);
    }
  }
};
</script>
<template lang="pug">
  .select-wrapper.mdb-select.md-form.job-search-control
    span.caret ▼
    input.select-dropdown(type='text' readonly='true' :placeholder="placeholder" @click="listOptions")
    ul.dropdown-content.select-dropdown.w-100.multiple-select-dropdown(v-show="list" :id="component_id")
      span.search-wrap.ml-2
        .md-form.ml
          input.search.form-control.w-100.d-block(type='text', placeholder='Search here..', v-model="filter")
      div(v-for="option in filterOptions" :key="option.id")
        li(v-if="option.options" class="optgroup" )
          span {{ option.label }}
        li(v-if="option.options" v-for="subOption in option.options")
          input.form-check-input(:id="GUID('__' + subOption.id)" type='checkbox' v-model="selections" :value="subOption")
          label(:for="GUID('__' + subOption.id)")
          span.filtrable(@click="select(subOption)") {{ subOption.label }}
        li(v-if="!option.options"  )
          input.pl-20.form-check-input(:id="GUID(option.id)" type='checkbox' v-model="selections" :value="option")
          label.pl-20(:for="GUID(option.id)")
          span.filtrable.pl-0(@click="select(option)" ) {{ option.label }}

      button.btn-save.btn.btn-primary.btn-sm(v-show="list" type="button" @click="listOptions") OK
</template>
<style lang="scss" scoped>
ul li.active {
  background-color: darkgray;
}
.dropdown-content.select-dropdown.w-100.multiple-select-dropdown {
  display: block;
  opacity: 1;
  max-height: 300px;
  width: 350px !important;
}

.dropdown-content li > span {
    display: inline;
    padding-left: 0rem;
}

.dropdown-content li {
    padding-left: 0.5rem;
    padding-bottom: 0.5rem;
}

.ml{
  margin-left: 0.5rem;
}

.select-wrapper input.select-dropdown {
  width: calc(100% - 13px);
}

</style>
