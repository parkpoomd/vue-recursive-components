<template>
  <div class="p-10">
    <ul>
      <node-item v-for="node in nodes" :key="node.id" :node="node"></node-item>
    </ul>
  </div>
</template>

<script>
import NodeItem from './components/NodeItem.vue';

export default {
  name: 'App',

  data() {
    return {
      nodes: [
        {
          label: 'Folder 1',
          checked: false,
          children: [
            {
              label: 'Folder 1.1',
              checked: false,
              children: [{ label: 'Folder 1.1.1', checked: false }],
            },
          ],
        },
        {
          label: 'Folder 2',
          checked: false,
          children: [
            {
              label: 'Folder 2.1',
              checked: false,
            },
            {
              label: 'Folder 2.2',
              checked: false,
            },
            {
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

  components: { NodeItem },
};
</script>
