<!-- eslint-disable vue/no-mutating-props -->
<template>
  <li :data-id="item.id">
    <label class="checkbox">
      <input type="checkbox" v-model="item.checked" />
      <span
        :style="{ 'text-decoration': item.checked ? 'line-through' : 'none' }"
        >{{ item.content }}</span
      >
    </label>
    <a class="delete" @click="deleteItem"></a>
  </li>
</template>

<script>
export default {
  props: {
    item: Object
  },
  methods: {
    deleteItem () {
      this.$emit('delete', this.item.id)
    }
  }
}
</script>

<style lang="scss">
@import '@/assets/base.scss';

li {
  position: relative;
  padding-right: 2rem;
  @media (max-width: 575px) {
    padding-right: 0;
  }
  a.delete {
    position: absolute;
    opacity: 0;
    right: 0;
    top: 50%;
    transform: translateY(-50%);
    text-decoration: none;
    color: $dark;
    display: block;
    width: 1rem;
    height: 1rem;
    background: #000;
    background-image: url('https://i.imgur.com/7Q4RjFx.jpg');
    background-size: contain;
    cursor: pointer;
    @media (max-width: 575px) {
      opacity: 1;
      width: 12px;
      height: 12px;
    }
  }
  &:hover a.delete {
    opacity: 1;
  }
}
</style>
