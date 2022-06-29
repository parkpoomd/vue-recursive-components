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

    <div class="w-64">
      <label class="inline-flex items-center pl-6 pb-2">
        <input
          type="checkbox"
          :checked="statusEmployeeCheckAll"
          @input="handleCheckAllEmployee($event)"
        />
        <span class="ml-2">Select All</span>
      </label>
      <ul class="pl-6">
        <li
          class="flex items-center"
          v-for="employee in filterEmployees"
          :key="employee.id"
        >
          <label class="inline-flex items-center">
            <input
              type="checkbox"
              :checked="employee.checked"
              @input="handleCheckEmployee(employee, $event)"
            />
            <span class="ml-2">
              {{ employee.name }}
            </span>
          </label>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
import _ from 'lodash';
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

  computed: {
    statusEmployeeCheckAll() {
      return _.every(this.filterEmployees, ['checked', true]);
    },
    filterEmployees() {
      let items = this.employees;

      items = this.employees.map((employee) => {
        if (this.selected.includes(employee.nodeId)) {
          return { ...employee, checked: true };
        }
        return employee;
      });

      return items;
    },
  },

  methods: {
    handleCheckEmployee(employee, event) {
      const isChecked = event.target.checked;
      const { id } = employee;
      console.log(isChecked, id);
    },
    handleCheckAllEmployee(event) {
      const isChecked = event.target.checked;
      console.log(isChecked);
    },
    re(node, isChecked) {
      node.checked = isChecked;
      if (node.children && node.children.length) {
        node.children.forEach((child) => {
          this.re(child);
        });
      }
    },
    handleSelected(node) {
      this.callRecursively(node);
    },
    callRecursively(node) {
      if (node.checked) {
        const index = this.selected.findIndex((id) => id === node.id);
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
