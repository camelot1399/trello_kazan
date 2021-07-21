<template>
    <div class="pop-over">
        <div class="pop__header">
            <h3>Действия со списком</h3>
            <button class="closePop" @click="$emit('close', list.popOver = false)">
                <i class="fas fa-times"></i>
            </button>
        </div>

        <div class="pop__body">
            <ul class="pop__ul">
                <li class="pop__li" @click="$emit('addTask')">Добавить задачу</li>
            </ul>
        </div>
    </div>
</template>
<script>
export default {
    name: 'BoardPopOver',
    data() {
        return {

        }
    },
    props: {
        list: Array
    }
}
</script>
<style>
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

.pop__ul {
    margin: 0;
    padding: 0;
}

.pop__li {
    list-style-type: none;
}

.closePop {
    border: none;
    background: none;
    cursor: pointer;
}

.closePop:hover {
    background: #ccc;
}
</style>