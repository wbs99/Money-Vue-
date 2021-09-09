<template>
  <div class="tags">
    <ul class="current">
      <li v-for="tag in tagList" :key="tag.id"
          :class="{selected: selectedTags.indexOf(tag)>=0}"
          @click="toggle(tag)">
        <Icon name="car"/>
        {{ tag.name }}
      </li>
      <li>
        <div class="new">
          <button @click="createTag">新增标签</button>
        </div>
      </li>
    </ul>
  </div>

</template>

<script lang="ts">
import {Component} from 'vue-property-decorator';
import {mixins} from 'vue-class-component';
import TagHelper from '@/mixins/TagHelper';

@Component
export default class Tags extends mixins(TagHelper) {
  selectedTags: string[] = [];
  get tagList() {
    return this.$store.state.tagList;
  }

  created() {
    this.$store.commit('fetchTags');
  }

  toggle(tag:string) {
    const index = this.selectedTags.indexOf(tag);
    if (index >= 0) {
      this.selectedTags.splice(index, 1);
    } else {
      this.selectedTags.push(tag);
    }
    this.$emit('update:value', this.selectedTags);
  }

}
</script>

<style lang="scss" scoped>
.tags {
  border: 1px solid blue;
  max-height: 420px;
  overflow: auto;
  > .current {
    border: 1px solid red;
    display: flex;
    flex-wrap: wrap;
    padding: 16px 20px;
    > li {
      border: 1px solid black;
      width: 25%;
      display: flex;
      align-items: center;
      justify-content: center;
      flex-direction: column;
      border-radius: 10px;
      margin: 12px;
      .icon {
        width: 28px;
        height: 28px;
      }

      &.selected {
        background: #cde1d9;
      }
    }
  }

  > .new {
    padding-top: 16px;

    button {
      background: transparent;
      border: none;
      color: #999;
      border-bottom: 1px solid;
      padding: 0 4px;
    }
  }
}
</style>