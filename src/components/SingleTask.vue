<template>
    <div id="task" :class="task.done ? 'task-inactive' : 'task-active'">
        <div id="info-area">
            <label :class="task.done ? 'inactive' : 'active'" >
                <input type="checkbox" @change="onCheck(task.id)" value="done" :checked="task.done" />{{task.content}}
            </label>
            <p id="task-details">{{task.author}} - {{task.dueDate}}</p>
        </div>
        <div id="mod-area">

            <button class="mdc-button mdc-button--outlined" v-show="!task.done && editEnable === false" @click="editEnable = !editEnable">
              <span class="mdc-button__ripple"></span>
              <span class="mdc-button__label">Edit</span>
            </button>

            <button class="mdc-button mdc-button--outlined" id="delete-btn" v-show="task.done" @click="onDelete(task.id)">
              <span class="mdc-button__ripple"></span>
              <span class="mdc-button__label">Delete</span>
            </button>

        </div>
        <EditTask v-if="editEnable" :oldTask="task" @close-edit-form="editEnable = !editEnable" @edit-task="editTask" />
    </div>
</template>

<script>
import EditTask from './EditTask.vue';
export default{
    name: 'SingleTask',
    props: {
        task: Object
    },
    components: {
        EditTask
    },
    data(){
        return {
            editEnable: false
        }
    },
    methods: {
        onCheck(id){
            this.$emit('checkbox-changed', id);
        },
        onDelete(id){
            this.$emit('delete-task', id);
        },
        onEdit(id){
            console.log("Edit Toggle: ", id);
        },
        editTask(newTask){
            //console.log(newTask);
            this.$emit('edit-task', newTask);
            this.editEnable = !this.editEnable;
        }
    },
    emit: ['edit-task']
}
</script>

<style scoped>
p{
    margin: 0;
}
.task-active{
    padding: 0.5rem;
    display: flex;
    border-bottom: 1px solid rgb(195, 195, 195);
    background-color: white;
    font-size: 14px;
    flex-wrap: wrap;
}
.task-inactive{
    padding: 0.25rem 0.5rem;
    display: flex;
    font-size: 14px;
    flex-wrap: wrap;
}
.active{
    color: rgb(63, 195, 248);
}
.inactive{
    color: grey;
    text-decoration: line-through;
}
#info-area{
    display: flex;
    width: 80%;
}
#task-details{
    margin-left: 1rem;
    color: gray;
}
#mod-area{
    display: flex;
    width: 20%;
    justify-content: flex-end;
}
#edit-btn{
    background-color: gray;
    box-shadow: none;
}
#delete-btn{
    color: red;
}
</style>