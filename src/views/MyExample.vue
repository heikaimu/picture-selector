<template>
  <div>
    <FloatSelector
      :list="list"
      :defaultList="defaultList"
    />
  </div>
</template>

<script>
import FloatSelector from '../plugin/floatSelector';
export default {
  components: {
    FloatSelector
  },
  data() {
    return {
      list: [],
      defaultList: []
    };
  },
  mounted() {
    this.list = this.createList(15);
    this.defaultList = this.createList(3);
  },
  methods: {
    addNewList() {
      this.list.push(...this.createList(this.list.length));
    },
    createList(count) {
      const list = [];
      for (let i = 0; i < count; i++) {
        list.push({
          id: i,
          src: require(`../assets/${i + 1}.jpg`),
          name: `Image${i + 1}`
        });
      }
      return list;
    }
  }
};
</script>

<style lang="scss" scoped>

</style>
