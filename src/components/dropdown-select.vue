<template>
  <div class="dropdown-select" @click.self="selectClicked">
    <div class="label">
      {{ label }}
      <span class="fa fa-chevron-down"></span>
    </div>
    <div class="items">
      <div
        v-for="(item, index) in items"
        v-bind:key="item.key"
        @click.stop="itemSelected(item.key, index)"
        :class="{
          disabled: activeItem === item.key,
        }"
      >
        {{ item.value }}
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "dropdown-select",
  props: {
    items: Array,
    activeItem: [String, Number],
    label: String,
  },
  methods: {
    selectClicked(event) {
      const item = event.target;
      const isActive = item.classList.contains("active");

      if (isActive) {
        return this.closeSelects();
      }
      this.closeSelects();
      item.classList.add("active");
    },
    closeSelects() {
      document
        .querySelectorAll(".dropdown-select")
        .forEach(select => select.classList.remove("active"));
    },
    itemSelected(key, index) {
      this.closeSelects();
      this.$emit("item-selected", { key, index });
    },
  },
};
</script>
<style lang="scss" rel="stylesheet/scss" scoped>
.dropdown-select {
  display: inline-block;
  vertical-align: top;
  background: rgba(51, 51, 51, 0.8);
  line-height: 34px;

  &.active,
  &:hover {
    background: rgb(51, 51, 51);
  }

  .label {
    pointer-events: none;
    height: 34px;
    padding: 0 10px;
    display: flex;
    justify-content: space-between;

    span {
      font-size: 12px;
      line-height: inherit;
      padding-left: 10px;
    }
  }

  .items {
    max-height: 200px;
    overflow-x: hidden;
    overflow-y: auto;

    > div {
      display: flex;
      justify-content: space-between;
      padding: 0 10px;

      &:hover {
        background: rgba(255, 255, 255, 0.1);
      }

      &.disabled {
        pointer-events: none;
        color: gray;
      }

      span {
        color: gray;
        font-size: 14px;
        padding-left: 10px;
      }
    }
  }

  &:not(.active) {
    .items {
      display: none;
    }
  }
}
</style>