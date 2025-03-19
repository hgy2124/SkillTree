<template>
  <div id="app" class="app">
    <div class="container">
      <div class="tree">
        <Node
          v-for="(node, index) in skillTree"
          :key="index"
          :node="node"
          :index="index"
          :is-root="true"
          @delete-node="deleteNode"
          @add-child="addChild"
          @update-name="updateName"
          @edit-node="editNode"
        ></Node>
      </div>
    </div>
  </div>
</template>

<script>
import { ref } from 'vue';
import Node from './components/Node.vue';

export default {
  name: 'App',
  components: {
    Node,
  },
  setup() {
    const skillTree = ref([
      {
        name: '기본 스킬',
        image: '',
        children: [],
      },
    ]);

    const addChild = (parent, direction) => {
      const newChild = {
        name: '새로운 스킬',
        image: '',
        children: [],
      };
      if (direction === 'right') {
        skillTree.value.push(newChild);
      } else {
        parent.children.push(newChild);
      }
    };

    const deleteNode = (index) => {
      skillTree.value.splice(index, 1);
    };

    const editNode = (node) => {
      const fileInput = document.createElement('input');
      fileInput.type = 'file';
      fileInput.accept = 'image/*';
      fileInput.onchange = (event) => {
        const file = event.target.files[0];
        if (file) {
          node.image = URL.createObjectURL(file);
        }
      };
      fileInput.click();
    };

    const updateName = (event, node) => {
      node.name = event.target.innerText;
    };

    return {
      skillTree,
      addChild,
      deleteNode,
      editNode,
      updateName,
    };
  },
};
</script>

<style scoped>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

.app {
  font-family: Arial, sans-serif;
  background: #020024;
  color: white;
  height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  overflow: auto;
}

.container {
  display: flex;
  flex-direction: row;
  justify-content: center;
  align-items: center;
}

.tree {
  display: flex;
  flex-direction: row;
  align-items: flex-start;
}
</style>