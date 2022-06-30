<template>
  <li>
    <div class="flex items-center">
      <button
        type="button"
        class="mr-2"
        v-if="hasChildren"
        @click="handleClickParent"
      >
        <svg
          xmlns="http://www.w3.org/2000/svg"
          class="h-5 w-5"
          viewBox="0 0 20 20"
          fill="currentColor"
        >
          <path
            fill-rule="evenodd"
            d="M7.293 14.707a1 1 0 010-1.414L10.586 10 7.293 6.707a1 1 0 011.414-1.414l4 4a1 1 0 010 1.414l-4 4a1 1 0 01-1.414 0z"
            clip-rule="evenodd"
          />
        </svg>
      </button>

      <div :class="`${!hasChildren ? 'pl-7' : ''}`">
        <label class="inline-flex items-center">
          <input
            type="checkbox"
            :checked="node.checked"
            @input="handleClickNode($event, node)"
          />
          <span class="ml-2">
            {{ node.label }}
          </span>
        </label>
      </div>
    </div>

    <ul v-show="hasChildren && showChildren" class="pl-6">
      <tree-list-item
        v-for="child in node.children"
        :key="child.id"
        :node="child"
        :handleSelected="handleSelected"
      ></tree-list-item>
    </ul>
  </li>
</template>

<script>
export default {
  name: 'TreeListItem',

  props: {
    node: Object,
    handleSelected: Function,
  },

  data() {
    return {
      showChildren: false,
      isNodeCheck: false,
    };
  },

  computed: {
    hasChildren() {
      return this.node.children && this.node.children.length;
    },
  },

  methods: {
    handleClickParent() {
      this.showChildren = !this.showChildren;
    },
    handleClickNode(event, node) {
      this.isNodeCheck = event.target.checked;

      this.setNodeRecursive(node);
      this.handleSelected(node);
    },
    setNodeRecursive(node) {
      node.checked = this.isNodeCheck;
      if (node.children && node.children.length) {
        node.children.forEach((children) => {
          this.setNodeRecursive(children);
        });
      }
    },
  },
};
</script>
