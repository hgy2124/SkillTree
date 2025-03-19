<template>
    <div class="node-container">
      <div class="node">
        <div class="image" @click="$emit('edit-node', node)">
          <img :src="node.image" v-if="node.image" />
        </div>
        <span contenteditable="true" @input="$emit('update-name', $event, node)" class="node-name">{{ node.name }}</span>
        <button class="delete-button" v-if="index !== 0 || !isRoot" @click="$emit('delete-node', index)">-</button>
      </div>
      <div class="buttons">
        <!-- 아래로 추가 버튼: 가장 하위 노드에서만 표시 -->
        <button v-if="isLeafNode(node)" class="add-button" @click="$emit('add-child', node, 'below')">+</button>
        <!-- 오른쪽으로 추가 버튼: 가장 하위 노드에서만 표시 -->
        <button v-if="isLeafNode(node)" class="add-button right" @click="$emit('add-child', node, 'right')">+</button>
      </div>
      <div v-if="node.children.length" class="children">
        <Node
          v-for="(child, childIndex) in node.children"
          :key="childIndex"
          :node="child"
          :index="childIndex"
          :is-root="false"
          @delete-node="deleteChild"
          @add-child="$emit('add-child')"
          @update-name="$emit('update-name')"
          @edit-node="$emit('edit-node')"
        ></Node>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    name: 'Node',
    props: {
      node: {
        type: Object,
        required: true,
      },
      index: {
        type: Number,
        required: true,
      },
      isRoot: {
        type: Boolean,
        default: false,
      },
    },
    methods: {
      deleteChild(childIndex) {
        this.node.children.splice(childIndex, 1);
      },
      isLeafNode(node) {
        return node.children.length === 0;
      },
    },
  };
  </script>
  
  <style scoped>
  .node-container {
    display: flex;
    flex-direction: column;
    align-items: center;
    margin: 10px;
  }
  
  .node {
    display: flex;
    flex-direction: column;
    align-items: center;
    background-color: rgba(255, 255, 255, 0.2);
    padding: 10px;
    border-radius: 10px;
  }
  
  .image {
    width: 100px;
    height: 100px;
    background-color: rgba(255, 255, 255, 0.3);
    border-radius: 10px;
    display: flex;
    justify-content: center;
    align-items: center;
    cursor: pointer;
  }
  
  img {
    max-width: 80px;
    max-height: 80px;
    object-fit: cover;
    border-radius: 5px;
  }
  
  .node-name {
    margin-top: 5px;
    text-align: center;
    color: white;
    font-size: 14px;
  }
  
  .add-button {
    background: transparent;
    color: #fff;
    border: none;
    font-size: 24px;
    cursor: pointer;
    margin-top: 5px;
  }
  
  .add-button.right {
    margin-left: 10px;
  }
  
  .delete-button {
    background: red;
    color: white;
    border: none;
    font-size: 20px;
    cursor: pointer;
    margin-top: 5px;
  }
  
  .children {
    display: flex;
    flex-direction: column;
    align-items: center;
  }
  </style>