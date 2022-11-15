<template>
    <div id="task" :class="task.done ? 'task-inactive' : 'task-active'">
        <div id="task-info">
            <label :class="task.done ? 'inactive' : 'active'" >
                <input type="checkbox" @change="onCheck(task.id)" value="done" :checked="task.done" />{{task.content}}<span id="task-details"
                >{{task.author}} - {{task.dueDate}}</span>
            </label>
        </div>

        <div id="task-nav">
            <button class="mdc-button mdc-button--outlined mdc-button--icon-leading mod-btn"
                v-show="!task.done && editEnable === false" @click="editEnable = !editEnable">
              <span class="mdc-button__ripple"></span>
              <i class="material-icons mdc-button__icon" aria-hidden="true">edit</i>
            </button>

            <button class="mdc-button mdc-button--outlined mdc-button--icon-leading mod-btn"
                v-show="task.done" @click="onDelete(task.id)">
              <span class="mdc-button__ripple"></span>
              <i class="material-icons mdc-button__icon" aria-hidden="true">delete</i>
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
    align-items: center;
}
.task-inactive{
    padding: 0.25rem 0.5rem;
    display: flex;
    font-size: 14px;
    flex-wrap: wrap;
    align-items: center;
}
.active{
    color: rgb(63, 195, 248);
}
.inactive{
    color: grey;
    text-decoration: line-through;
}
#task-info{
    display: flex;
    width: 80%;
}
#task-details{
    margin-left: 1rem;
    color: gray;
}
#task-nav{
    display: flex;
    width: 20%;
    justify-content: flex-end;
}
.mod-btn{
    padding: 0 2px 0 11px;
    min-width: 36px;
}
</style>
