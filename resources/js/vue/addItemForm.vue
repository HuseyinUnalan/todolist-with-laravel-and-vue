<template>
  <div class="addItem">
    <input type="text" v-model="item.name" />

    <button @click="addItem()" :class="[item.name ? 'active' : 'inactive', 'plus']">
      Add
    </button>

  </div>
</template>

<script>
import axios from 'axios';

export default {
  data: function () {
    return {
      item: {
        name: ""
      }
    }
  },
  methods: {
    addItem() {
      if (this.item.name == '') {
        return;
      }
      axios.post('api/item/store', {
        item: this.item
      })
        .then(response => {
          if (response.status == 201) {
            this.item.name = "";
            this.$emit('reloadlist');
          }
        })
        .catch(error => {
          console.log(error);
        })
    }
  }
}
</script>

<style scoped>
.addItem {
  display: flex;
  justify-content: center;
  align-items: center;
}

input {
  background: #f7f7f7;
  border: 0px;
  outline: none;
  padding: 5px;
  margin-right: 10px;
  width: 100%;
}

button {
  padding: 5px;
  border: 0px;
  border-radius: 5px;
}

.plus {
  font-size: 16px;
}

.active {
  background-color: #3dba54;
  color: white;
}

.inactive {
  color: #fff;
  background-color: gray;
}
</style>