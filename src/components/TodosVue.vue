<script>
import InputNew from "./InputVue.vue";
import { reactive } from "vue";

export default {
  components: {
    InputNew,
  },
  data() {
    return {
      boards: reactive([
        {
          id: crypto.randomUUID(),
          name: "tablero1",
          items: [
            {
              id: crypto.randomUUID(),
              title: "facture archivos",
            },
            {
              id: crypto.randomUUID(),
              title: "facture implemente",
            },
          ],
        },
        {
          id: crypto.randomUUID(),
          name: "tablero2",
          items: [
            {
              id: crypto.randomUUID(),
              title: "reports file",
            },
            {
              id: crypto.randomUUID(),
              title: "revads implemente",
            },
          ],
        },
      ]),
    };
  },
  methods: {
    handlerNewItem(text, board) {
      console.log(text, board.name, board.id);
      board.items.push({
        id: crypto.randomUUID(),
        title: text,
      });
    },
    handlernewBoard() {
      const name = prompt("name of the board");
      if (!!name) {
        this.boards.push({
          id: crypto.randomUUID(),
          name: name,
          items: [],
        });
      }
    },
    statrDrag(evt, board, item) {
      evt.dataTransfer.setData(
        "text/plain",
        JSON.stringify({ boardId: board.id, itemId: item.id })
      );
    },
    onDrop(evt, dest) {
      const { itemId, boardId } = JSON.parse(
        evt.dataTransfer.getData("text/plain")
      );
      const originBoard = this.boards.find((item) => item.id === boardId);
      const originItem = originBoard.items.find((item) => item.id === itemId);

      dest.items.push({ ...originItem }); //ingresando destructurado el objeto

      originBoard.items = originBoard.items.filter(
        (item) => item != originItem
      ); //filtrar el item que se arrastro o borrarlo del arreglo
    },
  },
};
</script>

<template>
  <nav>
    <ul>
      <li><a href="#" @click="handlernewBoard">Create table</a></li>
    </ul>
  </nav>

  <div class="board_container">
    <div class="boards">
      <div class="board" v-for="board in boards" :key="board.id">
        <div class="title" @drop="onDrop($event, board)" @dragover.prevent @dragenter.prevent>
          {{ board.name }}
          <InputNew @on-new-item="(text) => handlerNewItem(text, board)" />
        </div>
        <div class="items" v-for="item in board.items" :key="item.id">
          <div
            class="item"
            draggable="true"
            @dragstart="statrDrag($event, board, item)"
          >
            {{ item.title }}
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
nav{
  background-color: black;
  height: 30px;
  margin-bottom:10px ;
}

nav ul{
  width: 100%;
  margin: 0;
  padding: 0;
  display: flex;
}
nav ul li{
  color: white;
  padding-left: 5px;
  margin-top: 5px;
  text-align: center;
  border: solid white 1px;

}
nav ul li a:hover{
  color: aqua;
  
}
a{
  text-decoration: none;
  color: white;
  cursor: pointer; 
}


.title{
  margin-bottom: 10px;
  font-weight: bold;
}
.boards {
  display: flex;
  gap: 10px;
 
}

.board {
  border: solid 1px black;
  background: #efefef;
  padding: 10px;
}
.items {
  display: flex;
  flex-direction: column;
  margin-bottom: 5px;
}
.item {
  background: white;
  padding: 10px;
  box-sizing: border-box;
}
</style>
