<template>
    <div id="board">
        <div 
            class="board__list" 
            :data-board="list.name" 
            v-for="list in board" 
            :key="list.name" 
        >
            <div class="board__header">
                <h3>{{ list.h3 }}</h3>
                <button class="showMenuBoardButton" @click="list.popOver = !list.popOver">
                    <i class="fas fa-ellipsis-h"></i>
                </button>

                <div class="pop-over" v-if="list.popOver">
                    <div class="pop__header">
                        <h3>Действия со списком</h3>
                        <button class="closePop" @click="list.popOver = false">
                            <i class="fas fa-times"></i>
                        </button>
                    </div>

                    <div class="pop__body">
                        <ul>
                            <li 
                                @click="popAddTask"
                            >Добавить задачу</li>
                        </ul>
                    </div>
                </div>
            </div>
            
            <button @click="list.tasks.push({hash: Date.now(),textarea: ''})" class="addNewTaskButton">
                <i class="fas fa-plus"></i> Добавить карточку
            </button>
            
            <div 
                class="tasks" 
                ondrop="drop_handler(event)" 
                ondragover="dragover_handler(event)"
            >
                <div 
                    v-for="task in list.tasks" 
                    :key="task.hash" 
                    @mouseover="task.showEdit = true" 
                    @mouseleave="task.showEdit = false"
                    @mousedown="tapDiv"
                    @mouseup="dropDiv"
                    draggable="true"
                    :id="task.hash"
                    class="taskWrapper" 
                >
                    <textarea 
                        name="text" 
                        placeholder="Введите заголовок для этой задачи" 
                        v-model="task.textarea"
                    ></textarea>
                    <button class="taskEdit" v-if="task.showEdit">
                        <i class="far fa-edit "></i>
                    </button>
                </div>
            </div>
            
        </div>
    </div>
</template>
<script>
export default {
    name: 'Board',
    data() {
        return {
            board: [
                {popOver: false, name: 'newTask', h3: 'Нужно сделать', tasks: [{showEdit: false, hash: 1, textarea: '123'}]},
                {popOver: false, name: 'inProgress', h3: 'В процессе', tasks: [{showEdit: false, hash: 2, textarea: 'lfflf'}]},
                {popOver: false, name: 'done', h3: 'Готово', tasks: [{showEdit: false, hash: 3, textarea: 'лалалаа'}]},
            ]
        }
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
                console.log('DOMContentLoaded');
                // Найти элемент по id
                const elements = document.querySelectorAll(".taskWrapper");
                console.log(elements);
                elements.forEach(element => {
                    // Добавить обработчик события `dragstart`
                    element.addEventListener("dragstart", this.dragstart_handler);
                })
                
            });
        },
        popAddTask(e) {
            let board = this.board.filter(el => el.name === e.target.closest('.board__list').dataset.board);
            board[0].tasks.push({showEdit: false, hash: Date.now(), textarea: ''})
            e.target.closest('.pop-over').querySelector('.closePop').click();
        }
    },
    mounted() {
        console.log(Date.now());
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
.showMenuBoardButton,
.closePop {
    border: none;
    background: none;
    cursor: pointer;
}

.addNewTaskButton:hover,
.showMenuBoardButton:hover,
.closePop:hover {
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

.taskWrapper {
    position: relative;
    margin-top: 5px;
}

.taskEdit {
    position: absolute;
    top: 5px;
    right: 0;
    border: none;
    background: none;
    cursor: pointer;

}

.pop-over {
    position: absolute;
    top: 100%;
    right: -100%;
    background: white;
    border: 1px solid #ccc;
    z-index: 999;
    /* display: none; */
}
.isShow {
    display: block;
}

.pop__header {
    display: flex;
    justify-content: space-between;
    font-size: 11px;
    padding: 5px;
    border-bottom: 1px solid #ccc;
}
ul {
    margin: 0;
    padding: 0;
}

li {
    list-style-type: none;
}


</style>