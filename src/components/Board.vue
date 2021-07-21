<template>
    <div id="board">
        <div 
            class="board__list" 
            :data-board="list.name" 
            v-for="list in board" 
            :key="list.name">
            <div class="board__header">
                <h3>{{ list.h3 }}</h3>
                <button class="showMenuBoardButton" @click="list.popOver = !list.popOver">
                    <i class="fas fa-ellipsis-h"></i>
                </button>

                <BoardPopOver 
                    :list="list" 
                    v-if="list.popOver"
                    v-on:close="$event"
                    v-on:addTask="popAddTask($event)"
                />
                
            </div>
            

            <button @click="popAddTask" class="addNewTaskButton">
                <i class="fas fa-plus"></i> Добавить карточку
            </button>
            
            <div 
                class="tasks" 
                ondrop="drop_handler(event)" 
                ondragover="dragover_handler(event)">

                <Task  
                    v-for="task in list.tasks" 
                    :key="task.hash"
                    :id="task.hash"
                    :task="task"
                    :list="list"
                    v-on:deleteT="deleteTask($event)"
                />
                
            </div>
            
        </div>
    </div>
</template>
<script>
import BoardPopOver from './PopUp/BoardPopOver.vue';
import Task from './Task.vue';

export default {
    name: 'Board',
    data() {
        return {
            board: [
                {popOver: false, name: 'newTask', h3: 'Нужно сделать', tasks: [{showEdit: false, popOver: false, hash: 1, textarea: '123'}]},
                {popOver: false, name: 'inProgress', h3: 'В процессе', tasks: []},
                {popOver: false, name: 'done', h3: 'Готово', tasks: []},
            ]
        }
    },
    components: {
        BoardPopOver,
        Task
    },
    methods: {
        showEdit(e) {
            console.log('mouseover');
            console.log(e);
        },
        tapDiv(e) {
            console.log('tap');
            console.log(e);
        },
        dropDiv(e) {
            console.log('drop');
            console.log(e);
        },
        dragover_handler(ev) {
            ev.preventDefault();
            ev.dataTransfer.dropEffect = "move";
        },
        drop_handler(ev) {
            ev.preventDefault();
            const data = ev.dataTransfer.getData("text/plain");
            ev.target.appendChild(document.getElementById(data));
        },
        dragstart_handler(ev) {
            console.log('dragstart_handler');

            ev.dataTransfer.setData("text/plain", ev.target.id);
            ev.dataTransfer.setData("text/plain", ev.target.innerText);
            ev.dataTransfer.setData("text/html", ev.target.outerHTML);

            ev.dataTransfer.dropEffect = "move";    
        },
        initDND() {
            console.log('initDND');
            window.addEventListener('DOMContentLoaded', () => {
                const elements = document.querySelectorAll(".taskWrapper");
                elements.forEach(element => {
                    // Добавить обработчик события `dragstart`
                    element.addEventListener("dragstart", this.dragstart_handler);
                })
                
            });
        },
        popAddTask(e) {
            let board = this.board.filter(el => el.name === e.target.closest('.board__list').dataset.board);
            board[0].tasks.push({showEdit: false, hash: Date.now(), textarea: ''})
            let parent = e.target.closest('.board__list');
            let closePop = parent.querySelector('.closePop');
            console.log(closePop);
            if (closePop !== null) {
                closePop.click();
            }
            
        },
        deleteTask({hash, boardName}) {
            let filter = this.board.filter(el => el.name === boardName);
            let arr = this.removeItemFromArray(filter[0].tasks, hash);
            let index = filter[0].tasks.indexOf(arr[0]);
            filter[0].tasks.splice(index, 1);
        },
        removeItemFromArray(arr, value) {
            return arr.filter(el => el.hash === value);
        }
    },
    mounted() {
        // console.log(Date.now());
        // setTimeout(() => {
        //     this.initDND();
        // }, 2000);
    }
}
</script>
<style>
h3 {
    margin: 5px;
}
#board {
    display: flex;
}

.board__header {
    display: flex;
    justify-content: space-between;
    position: relative;
}

.board__list {
    background: #ebecf0;
    margin: 0 5px;
    padding: 5px;
    box-sizing: border-box;
}

.addNewTaskButton,
.showMenuBoardButton {
    border: none;
    background: none;
    cursor: pointer;
}

.addNewTaskButton:hover,
.showMenuBoardButton:hover {
    background: #ccc;
}

textarea {
    background: white;
    resize: none;
    border: none;
    box-shadow: 1px 2px 5px rgba(1,1,1, 0.3);
    border-radius: 5px;
}
textarea:active, :hover, :focus {
    outline: 0;
    outline-offset: 0;
}
</style>