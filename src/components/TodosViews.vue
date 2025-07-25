<script setup>
import { reactive } from 'vue';
import InputNew from './InputNew.vue';

let boards = reactive([
  {
    id: crypto.randomUUID(),
    name: 'Tablero 1',
    items: [
      { id: 1, text: 'Todo 1', completed: false },
      { id: 2, text: 'Todo 2', completed: true }
    ]
  },
  {
    id: crypto.randomUUID(),
    name: 'Tablero 2',
    items: [
      { id: 1, text: 'Random 1', completed: false },
      { id: 2, text: 'Complete 2', completed: true }
    ]
  },
  {
    id: crypto.randomUUID(),
    name: 'Tablero 3',
    items: [
      { id: 1, text: 'Resolver bug', completed: false },
      { id: 2, text: 'Resuelve todo', completed: true }
    ]
  }
]);

function handleNewItem(text, board) {
  board.items.push({
    id: crypto.randomUUID(),
    text: text,
    completed: false
  });
}
function handelNewBoard() {
    const name = prompt("Introduce el nombre del nuevo tablero:");
    if (name.trim() === "") {
        alert("El nombre del tablero no puede estar vacío.");
        return;
    }else {
        boards.push({
            id: crypto.randomUUID(),
            name: name,
            items: []
        });
    }

}

function startDrag( evt , board , item) {
    evt.dataTransfer.setData("text/plain", JSON.stringify({ boardId: board.id, itemId: item.id }));


}

function onDrag(evt , dest ) {
    const data = JSON.parse(evt.dataTransfer.getData("text/plain"));
    const sourceBoard = boards.find(board => board.id === data.boardId);
    const origin = sourceBoard.items.find(item => item.id === data.itemId);

    dest.items.push({ ...origin});
    sourceBoard.items = sourceBoard.items.filter((item) => item !== origin);
    
}

    
</script>

<template>
  <nav>
    <ul>
      <li><a href="crear tablero" @click.prevent="handelNewBoard">Crear tablero</a></li>
    </ul>
  </nav>

  <div class="boards-container">
    <div class="boards">
      <div class="board" @drop="onDrag($event , board )" @dragover.prevent @dragenter.prevent v-for="board in boards" :key="board.id">
        <h3>{{ board.name }}</h3>
        <InputNew @onNewItem="(text) => handleNewItem(text, board)" />
        <div class="items">
          <div
            v-for="item in board.items"
            :key="item.id"
            class="item"
            draggable="true"
            @dragstart="startDrag($event, board, item)"
          >
            {{ item.text }}
          </div>
        </div>
      </div> 
    </div> 
  </div>
</template>

<style scoped>
.boards-container {
  padding: 20px;
}
.boards {
  display: flex;
  gap: 20px;
}
.board {
  background: #efefef;
  padding: 15px;
  border-radius: 5px;
  width: 250px;
}
.items {
  margin-top: 10px;
}
.item {
  background-color: white;
  padding: 10px;
  margin-bottom: 5px;
  border-radius: 4px;
}
</style>
