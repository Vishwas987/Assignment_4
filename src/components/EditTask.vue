<template>
    <div id="edit-task">
            <form>
                <div>
                    <label class="mr-1">Due Date:</label>
                    <input class="input-field" type="date" v-model.lazy="task.dueDate" required />
                </div>

                <div>
                    <label class="mr-1">Content:</label>
                    <input class="input-field" type="text" placeholder="Enter Task" v-model.lazy="task.content" required />
                </div>

                <div>
                    <button class="mdc-button mdc-button--outlined mr-1" @click.prevent="submit">
                      <span class="mdc-button__ripple"></span>
                      <span class="mdc-button__label">Save</span>
                    </button>

                    <button class="mdc-button mdc-button--outlined"  @click.prevent="$emit('close-edit-form')">
                      <span class="mdc-button__ripple"></span>
                      <span class="mdc-button__label">Cancel</span>
                    </button>
                </div>
            </form>
        </div>
</template>

<script>
export default{
    name: 'EditTask',
    props: {
        oldTask: Object
    },
    data(){
        return {
            task: {
                dueDate: this.oldTask.dueDate,
                content: this.oldTask.content,
            }
        }
    },
    methods: {
        submit(){
            let tmp_task = {
                content: this.task.content,
                dueDate: this.task.dueDate,
                _id: this.oldTask._id
            };

            this.$emit('edit-task', tmp_task);
        }
    }
}
</script>

<style scoped>
.mr-1{
    margin-right: 0.5rem;
}
#edit-task{
    width: 100%;
}
form{
    display: flex;
    justify-content: space-between;
    padding: 0.25rem;

}
.input-field{
    height: 90%;
}
</style>
