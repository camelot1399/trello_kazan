<template>
    <div 
        @mouseover="task.showEdit = true" 
        @mouseleave="task.showEdit = false"
        draggable="true"
        class="taskWrapper"
        ondragstart="event.dataTransfer.setData('text/plain',null)"
        >
        <textarea 
            name="text" 
            placeholder="Введите заголовок для этой задачи" 
            v-model="task.textarea">
        </textarea>
        <button class="taskEdit" v-if="task.showEdit" @click="task.popOver = !task.popOver">
            <i class="far fa-edit"></i>
        </button>

        <div class="pop-over" v-if="task.popOver">
            <div class="pop__header">
                <h3>Действия со списком</h3>
                <button class="closePop" @click="task.popOver = false">
                    <i class="fas fa-times"></i>
                </button>
            </div>

            <div class="pop__body">
                <ul>
                    <li @click="$emit('deleteT', {hash: task.hash, boardName: list.name})">Удалить</li>
                    <li @click="popupMove = !popupMove">Переместить</li>
                </ul>
            </div>
            
            <div class="popupMoveBlock" v-if="popupMove">
                <div class="pop__header">
                    <h3>Перемещение в board</h3>
                    <button class="closePop" @click="popupMove = !popupMove">
                        <i class="fas fa-times"></i>
                    </button>
                </div>

                <div class="pop__body">
                    <select name="select"> <!--Supplement an id here instead of using 'name'-->
                        <option value="newTask" selected>Нужно сделать</option>
                        <option value="inProgress" >В процессе</option>
                        <option value="done">Готово</option>
                    </select>
                </div>
            </div>
        </div>
    </div>
</template>
<script>
export default {
    name: 'Task',
    data() {
        return {
            popupMove: false
        }
    },
    props: {
        task: Object,
        list: Object
    },
    mounted() {
        console.log(this.list);
    }
}
</script>
<style>
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
</style>