<template>
    <div>
        <div class="checklist_block">
            <h3 class="checklist_head">TASKS</h3>
            <div class="d-flex">
                <div class="mb-3" >
                    <input type="text" class="form-control" style="width: 500px;" id="newTaskInput" placeholder="new task" v-model="newTask">
                </div>
                <div>
                    <div class="btn btn-primary pt-6" @click="addTask()">add</div>
                </div>
            </div>
            <table class="table">
                <thead>
                    <tr>
                        <th>
                            <div class="sort" @click="reverseList">
                                <img width="15px" src="/img/sort.png" alt="sort">
                            </div>
                        </th>
                        <th>task</th>
                        <th>action</th>
                    </tr>
                </thead>
                <tbody>
                    <tr v-for="(item, index) in list" :key="index">
                        <td >                        
                            <input type="checkbox" v-model="item.done"  @change="saveList">
                        </td>
                        <td>
                            <div 
                                :class="{'text-decoration-line-through': item.done}" 
                                title="double click to edit"
                                v-if="!item.isEditing"
                                @dblclick="item.isEditing = !item.isEditing"
                            >
                                {{ item.desc }}
                            </div>
                            <div>
                                <input 
                                    type="text" 
                                    v-if="item.isEditing"
                                    v-model="item.desc"
                                    @change="item.isEditing = !item.isEditing"
                                >
                            </div>
                        </td>
                        <td>
                            <div class="deleteBtn" @click="deleteTask(index)">
                                <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="red"  class="bi bi-trash" viewBox="0 0 16 16">
                                    <path d="M5.5 5.5A.5.5 0 0 1 6 6v6a.5.5 0 0 1-1 0V6a.5.5 0 0 1 .5-.5m2.5 0a.5.5 0 0 1 .5.5v6a.5.5 0 0 1-1 0V6a.5.5 0 0 1 .5-.5m3 .5a.5.5 0 0 0-1 0v6a.5.5 0 0 0 1 0z"/>
                                    <path d="M14.5 3a1 1 0 0 1-1 1H13v9a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2V4h-.5a1 1 0 0 1-1-1V2a1 1 0 0 1 1-1H6a1 1 0 0 1 1-1h2a1 1 0 0 1 1 1h3.5a1 1 0 0 1 1 1zM4.118 4 4 4.059V13a1 1 0 0 0 1 1h6a1 1 0 0 0 1-1V4.059L11.882 4zM2.5 3h11V2h-11z"/>
                                </svg>
                            </div>
                        </td>
                        
                    </tr>
                </tbody>
            </table>
        </div>
    </div>
</template>

<script setup>
    import { computed, onMounted, ref } from 'vue';

    const list = ref([]);

    const newTask = ref('');

    function addTask() {
        if (newTask.value.trim() == "") {
            alert('Инпут не должен быть пустым'); 
            return false;           
        }

        list.value.push({
            done: false,
            desc: newTask.value
        });

        saveList();
        newTask.value = '';
    }

    function deleteTask(index) {
        console.log(index)
        list.value.splice(index, 1);
        saveList();
    }

    function reverseList() {
        list.value.reverse();
    };

    function saveList() {
        localStorage.setItem('taskList', JSON.stringify(list.value));
    }

    function loadList() {
        const savedList = localStorage.getItem('taskList');

        if (savedList) {
            list.value = JSON.parse(savedList);            
        }
    }

    onMounted(() => {
        loadList()
    })

    
</script>

<style scoped>
    .checklist_block {
        width: 600px;
        min-height: 600px;
        border: 2px solid orange;
        padding: 20px;
        border-radius: 5%;
    }

    .checklist_head {
        color:    #ee4343;
    }

    .deleteBtn {
        color: red;
        cursor: pointer;
    }

    .deleteBtn:hover {
        color: black;
    }

    .sort {
        cursor:  pointer;
    }

   
</style>