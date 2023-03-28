<script setup>
import { reactive } from "vue";
import imputNew from "../components/imputNew.vue";
components: { imputNew }

let boards = reactive([
    {
        id: crypto.randomUUID(),
        name: 'Tablero 1',
        items: [
            {
                id: crypto.randomUUID(),
                tittle: "Features de archivos",
            },
            {
                id: crypto.randomUUID(),
                tittle: "Terminar el turno",
            }
        ]
    },
    {
        id: crypto.randomUUID(),
        name: 'Tablero 2',
        items: [
            {
                id: crypto.randomUUID(),
                tittle: "ir al gym",
            },
            {
                id: crypto.randomUUID(),
                tittle: "tomar aminoacidos",
            }
        ]
    }
])

function handleNewItem(text, board) {
    console.log(text.value, board.id, board.name);
    board.items.push({
        id: crypto.randomUUID(),
        tittle: text.value,
    });
}

function handleNewBoard() {
    const nombreTab = prompt("Ingrese el nombre del tablero");
    if (!!nombreTab) {
        boards.push({
            id: crypto.randomUUID(),
            name: nombreTab,
            items: [],
        });
    }
}

function starDrag(event, board, item) {
    event.dataTransfer.setData('text/plain', JSON.stringify({ boardId: board.id, itemId: item.id }))
}

function onDrop(event, destino) {
    // Obtiene el tablero origen del item asi como sus ids y nombres
    const { boardId, itemId } = JSON.parse(event.dataTransfer.getData("text/plain"))
    // console.log(boardId, itemId);
    const originBoard = boards.find(item => item.id === boardId);
    const originItem = originBoard.items.find(item => item.id === itemId);
    // console.log(originBoard.name, originItem.tittle)

    // se agrega el elemento al tablero donde se movio y se elimina del tablero origen
    destino.items.push({ ...originItem });
    originBoard.items = originBoard.items.filter((item) => item != originItem);
}
</script>

<template>
    <nav>
        <ul>
            <li><a href="#" @click.prevent="handleNewBoard">Crear tablero</a></li>
        </ul>
    </nav>
    <div class="boards-container">
        <div class="boards">
            <div class="board" @drop="onDrop($event, board)" @dragover.prevent @dragenter.prevent v-for="board in boards"
                :key="board.id">
                <div>{{ board.name }}</div>
                <imputNew @on-new-item="(text) => handleNewItem(text, board)" />
                <div class="items">
                    <div class="item" draggable="true" @dragstart="starDrag($event, board, item)"
                        v-for="item in board.items" :key="item.id">
                        {{ item.tittle }}
                    </div>

                </div>
            </div>
        </div>
    </div>
</template>

<!-- la palabra scoped es para que los estilos solo sean aplicables en el componente que los declara -->
<style scoped>

nav {
background-color: black;
margin-bottom: 10px;
}

nav ul {
    list-style: none;
    padding: 0;
    margin:0;
    display: flex;
}

nav ul li a {
    display: block;
    padding: 10px;
    color: white;
    text-decoration: none;
}
.boards {
    display: flex;
    gap: 10px;
}

.board {
    background: #efefef;
    padding: 10px;
}

.items {
    display: flex;
    flex-direction: column;
    gap: 5px;
}

.item {
    background-color: white;
    padding: 10px;
    box-sizing: border-box;
}
</style>