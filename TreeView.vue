<template>
  <li>
    <div
      :class="{'root-content': isFolder, 'leaf-content': !isFolder, open : open}">
      <b-button-group>
        <b-button @click="toggle" @dblclick="changeType">
          <i class="icon-folder item-icon" v-if="isFolder && !open"></i>
          <i class="icon-folder-alt item-icon" v-else-if="isFolder && open"></i>
          <i class="icon-doc item-icon" v-else></i>
          {{ model.name }}
        </b-button>
        <b-button v-on:click="$emit('add-item', model)" v-if="!model.disableAdd">
          <i class="icon-plus item-icon"></i>
        </b-button>
        <b-button v-on:click="$emit('edit-item', model)" v-if="!model.disableEdit">
          <i class="icon-pencil item-icon"></i>
        </b-button>
        <b-button variant="danger" v-on:click="$emit('delete-item', model)" v-if="!model.disableDelete">
          <i class="icon-trash item-icon"></i>
        </b-button>
      </b-button-group>
    </div>
    <ul v-show="open" v-if="isFolder" class="child-container">
      <tree-view
        class="item"
        :class="{'root-li': isFolder, 'leaf-li': !isFolder}"
        v-for="(model, index) in model.children"
        v-on:edit-item="$emit('edit-item', model)"
        v-on:add-item="$emit('add-item', model)"
        v-on:delete-item="$emit('delete-item', model)"
        :key="index"
        :model="model">
      </tree-view>
    </ul>
  </li>
</template>

<script>

import Vue from 'vue';
import FontAwesome from "../../views/icons/FontAwesome";

export default {
  name: "TreeView",
  components: {FontAwesome},
  props: {
    model: Object
  },
  data: function () {
    return {
      open: false
    }
  },
  computed: {
    isFolder: function () {
      return this.model.children &&
        this.model.children.length
    }
  },
  methods: {
    toggle: function () {
      if (this.isFolder) {
        this.open = !this.open
      }
    },
    changeType: function () {
      return;
      if (!this.isFolder) {
        Vue.set(this.model, 'children', [])
        this.addChild()
        this.open = true
      }
    },
    addChild: function () {
      this.model.children.push({
        name: 'new stuff'
      })
    }
  }
}
</script>



<style lang="scss">
  @import "../../components/tree-view/styles";
</style>
