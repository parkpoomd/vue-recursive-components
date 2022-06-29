<template>
  <div class="p-10 flex">
    <ul class="pr-6 border-r-2">
      <node-item
        v-for="node in nodes"
        :key="node.id"
        :node="node"
        :handleSelected="handleSelected"
      ></node-item>
    </ul>

    <ul class="pl-6">
      <li
        class="flex items-center"
        v-for="employee in filterEmployees"
        :key="employee.id"
      >
        <label class="inline-flex items-center">
          <input type="checkbox" :checked="employee.checked" />
          <span class="ml-2">
            {{ employee.name }}
          </span>
        </label>
      </li>
    </ul>
  </div>
</template>

<script>
import NodeItem from './components/NodeItem.vue';

export default {
  name: 'App',

  data() {
    return {
      selected: [],
      employees: [
        { id: '1', name: 'name 1', checked: false, nodeId: '1' },
        { id: '2', name: 'name 1', checked: false, nodeId: '1' },
        { id: '3', name: 'name 1.1', checked: false, nodeId: '1.1' },
        { id: '4', name: 'name 1.1.1', checked: false, nodeId: '1.1.1' },
        { id: '5', name: 'name 2', checked: false, nodeId: '2' },
        { id: '6', name: 'name 2.1', checked: false, nodeId: '2.1' },
        { id: '7', name: 'name 2.1', checked: false, nodeId: '2.1' },
        { id: '8', name: 'name 2.2', checked: false, nodeId: '2.2' },
        { id: '9', name: 'name 2.3', checked: false, nodeId: '2.3' },
        { id: '10', name: 'name 2.3', checked: false, nodeId: '2.3' },
      ],
      nodes: [
        {
          id: '1',
          label: 'Folder 1',
          checked: false,
          children: [
            {
              id: '1.1',
              label: 'Folder 1.1',
              checked: false,
              children: [
                { id: '1.1.1', label: 'Folder 1.1.1', checked: false },
              ],
            },
          ],
        },
        {
          id: '2',
          label: 'Folder 2',
          checked: false,
          children: [
            {
              id: '2.1',
              label: 'Folder 2.1',
              checked: false,
            },
            {
              id: '2.2',
              label: 'Folder 2.2',
              checked: false,
            },
            {
              id: '2.3',
              label: 'Folder 2.3',
              checked: false,
            },
          ],
        },
      ],
    };
  },

  created() {
    // fetch('https://jsonplaceholder.typicode.com/photos')
    //   .then((response) => response.json())
    //   .then((json) => {
    //     this.nodes = [
    //       {
    //         id: 12494,
    //         label: 'Folder 1',
    //         checked: false,
    //         children: json
    //           .map((element) => ({
    //             id: element.id,
    //             label: element.title.substring(0, 10),
    //             checked: false,
    //           }))
    //           .slice(0, 500),
    //       },
    //     ];
    //   });
  },

  computed: {
    filterEmployees() {
      console.log('filter');
      let items = this.employees;

      if (this.selected.length === 0) {
        items = items.map((item) => ({ ...item, checked: false }));
      } else {
        this.selected.map((id) => {
          items = this.employees.map((employee) => {
            if (employee.nodeId === id) {
              return { ...employee, checked: true };
            }
            return employee;
          });
        });
      }

      return items;
    },
  },

  methods: {
    handleSelected(node) {
      this.callRecursively(node);
      console.log(this.selected);
    },
    callRecursively(node) {
      if (node.checked) {
        const index = this.selected.findIndex((element) => element === node.id);
        if (index === -1) {
          this.selected = [...this.selected, node.id];
        }
      } else {
        this.selected = this.selected.filter((id) => id !== node.id);
      }
      if (node.children && node.children.length) {
        node.children.forEach((childNode) => {
          this.callRecursively(childNode);
        });
      }
    },
  },

  components: { NodeItem },
};
</script>
