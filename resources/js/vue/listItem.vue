<template>
  <div class="item">
    <input type="checkbox" @change="updateCheck()" v-model="item.completed">
    <span :class="[item.completed ? 'completed' : '', 'itemText']">{{ item.name }}</span>
    <button @click="removeItem()" class="trashcan">Delete</button>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  props: ['item'],
  methods: {
    updateCheck() {
      axios.put('api/item/' + this.item.id, {
        item: this.item
      })
        .then(response => {
          if (response.status == 200) {
            this.$emit('itemChanged')
          }
        })
        .catch(error => {
          console.log(error);
        })
    },
    removeItem() {
      axios.delete('api/item/' + this.item.id)
        .then(response => {
          if (response.status == 200) {
            this.$emit('itemChanged');
          }
        })
        .catch(error => {
          console.log(error);
        })
    }
  }
}
</script>

<style>
.completed {
  text-decoration: line-through;
  color: #999999;
}

.itemText {
  width: 100%;
  margin-left: 20px;
}

.item {
  display: flex;
  justify-content: center;
  align-items: center;
}

.trashcan {
  background: #e6e6e6;
  border: none;
  color: ff000;
  outline: none;
}
</style>