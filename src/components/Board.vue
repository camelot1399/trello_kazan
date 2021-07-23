<template>
    <div id="board">
        <div class="board__list dropzone" :data-board="list.name" v-for="list in board" :key="list.name">
            <div class="board__header">
                <h3>{{ list.h3 }}</h3>
                <button class="showMenuBoardButton" @click="list.popOver = !list.popOver">
                    <i class="fas fa-ellipsis-h"></i>
                </button>

                <BoardPopOver 
                    :list="list" 
                    v-if="list.popOver"
                    v-on:close="list.popOver = !list.popOver"
                    v-on:addTask="popAddTask"
                />
            </div>
            
            <button @click="popAddTask" class="addNewTaskButton">
                <i class="fas fa-plus"></i> Добавить карточку
            </button>
            
            <div class="tasks" >
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
            ],
            dragged: null,
        }
    },
    components: {
        BoardPopOver,
        Task
    },
    methods: {
        initDND() {
            document.addEventListener("drag", function(event) {
                // console.log(event);
            }, false);  
            document.addEventListener("dragstart", function(event) {
            // store a ref. on the this.dragged elem
            this.dragged = event.target;
            // make it half transparent
            event.target.style.opacity = .5;
            }, false);

            document.addEventListener("dragend", function(event) {
            // reset the transparency
            event.target.style.opacity = "";
            }, false);

            /* events fired on the drop targets */
            document.addEventListener("dragover", function(event) {
            // prevent default to allow drop
            event.preventDefault();
            }, false);
            document.addEventListener("dragenter", function(event) {
            // highlight potential drop target when the draggable element enters it
            if (event.target.classList.contains('dropzone')) {
                event.target.style.background = "purple";
            }

            }, false);

            document.addEventListener("dragleave", function(event) {
            // reset background of potential drop target when the draggable element leaves it
            if (event.target.classList.contains('dropzone')) {
                event.target.style.background = "";
            }

            }, false);

            document.addEventListener("drop", function(event) {
            // prevent default action (open as link for some elements)
                event.preventDefault();
                // move this.dragged elem to the selected drop target
                console.log('сброс груза');

                if (!event.target.classList.contains('dropzone')) {
                    return
                }

                event.target.style.background = "";
                this.dragged.parentNode.removeChild( this.dragged );
                event.target.appendChild( this.dragged );
            }, false);
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
        setTimeout(() => {
            this.initDND();
        }, 2000);
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