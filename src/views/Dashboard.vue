<template>
    <div>
        <NavInBoard />
        <Board />
    </div>
</template>
<script>
import NavInBoard from '../components/Nav/inBoard.vue';
import Board from '../components/Board.vue';

export default {
    name: 'Dashboard',
    data() {
        return {
            name: 'Dashboard',
        }
    },
    components: {
        NavInBoard,
        Board
    }
}
</script>
<style>

</style>