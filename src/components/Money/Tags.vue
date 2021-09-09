<template>
  <div class="tags">
    <ul class="current">
      <li v-for="tag in tagList" :key="tag.id"
          :class="{selected:selectedTags.indexOf(tag)>=0}"
          @click="toggle(tag)">
        <Icon name="car" class="icon"/>
        {{ tag.name }}
      </li>
      <li>
        <div class="new">
          <button @click="createTag">添加</button>
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

  toggle(tag: string) {
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
  margin: 32px 0;
  height: 30vh;
  overflow: auto;

  > .current {
    overflow: auto;
    display: flex;
    flex-wrap: wrap;

    > li {
      width: 33.333%;
      display: flex;
      align-items: center;
      justify-content: center;
      flex-wrap: wrap;
      flex-direction: column;
      border-radius: 10px;
      font-size: 12px;
      font-weight: bolder;
      //padding: 8px 12px;
      .icon {
        width: 48px;
        height: 48px;
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