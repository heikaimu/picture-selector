<template>
  <div class="float-selector">
    <slot name="top" />
    <div class="card">
      <ul class="card-list">
        <li
          v-for="item in list"
          :key="item[rowKey]"
          class="card-item"
        >
          <Card
            :src="item.src"
            :checked.sync="item.checked"
            clickable
            @handleClick="flag => changeCardCheckedState(item, flag)"
          />
        </li>
      </ul>
    </div>
    <div class="selected-card">
      <ul class="card-list">
        <li
          v-for="item in checkedList"
          :key="item[rowKey]"
          class="card-item"
        >
          <Card
            :src="item.src"
            :checked.sync="item.checked"
            closeable
            @handleClose="handleClose(item)"
          />
        </li>
      </ul>
    </div>

    <slot name="bottom" />
  </div>
</template>

<script>
import Card from './Card';
export default {
  components: {
    Card
  },
  props: {
    list: {
      type: Array,
      default: () => []
    },
    defaultList: {
      type: Array,
      default: () => []
    },
    rowKey: {
      type: String,
      default: 'id'
    }
  },
  data() {
    return {
      checkedList: []
    };
  },
  watch: {
    defaultList: {
      handler: function(val) {
        this.checkedList = val;
      },
      immediate: true
    },
    checkedList: {
      handler: function(val) {
        const keys = val.map(item => item[this.rowKey]);
        this.list.forEach(item => {
          this.$set(item, 'checked', keys.includes(item[this.rowKey]));
        });
      },
      immediate: true
    }
  },
  methods: {
    changeCardCheckedState(item, flag) {
      if (flag) {
        const card = { ...item };
        delete card.checked;
        this.checkedList.push(card);
      } else {
        const index = this.checkedList.findIndex(card => card[this.rowKey] === item[this.rowKey]);
        this.checkedList.splice(index, 1);
      }
    },
    handleClose(item) {
      const index = this.checkedList.findIndex(card => card[this.rowKey] === item[this.rowKey]);
      this.checkedList.splice(index, 1);
    }
  }
};
</script>

<style lang="scss" scoped>
.float-selector {
  width: 1000px;
  margin: 0 auto;
  background-color: #fff;
  box-shadow: 0 0 5px #ccc;
  .card {
    height: 400px;
    overflow-y: auto;
    .card-list {
      padding: 5px;
      display: flex;
      flex-wrap: wrap;
      .card-item {
        margin: 5px;
      }
    }
  }
  .selected-card {
    background-color: #f2f2f2;
    .card-list {
      padding: 5px;
      display: flex;
      flex-wrap: wrap;
      .card-item {
        margin: 5px;
      }
    }
  }
}
</style>
