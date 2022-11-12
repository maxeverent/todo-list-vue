<template>
    <StackLayout class="body-task">
        <FlexboxLayout style="justify-content: space-between;" >
            <StackLayout class="padding-items">
                <Label v-model="task.title" class="label" style="font-weight: bold; font-size: 17px;" />
                <TextView v-model="task.content" class="label-content" editable="false" />                
                <Label v-if="task.statusTask" text="Completed" class="label"/>
            </StackLayout>
            <StackLayout class="padding-items" style="padding: 5px;" >
                <Button v-if="task.statusTask" text="In work" @tap="complete(id)" class="buttons-task" />
                <Button v-else text="Complete" @tap="complete(id)" class="buttons-task" />
                <Button text="Edit" @tap="$navigateTo(EditTask, {props: {id, task}})" class="buttons-task" />
            </StackLayout>
        </FlexboxLayout>
    </StackLayout>
</template>

<script>

import EditTask from './EditTask.vue'

const appSettings = require('@nativescript/core/application-settings');

export default {
    data() {
        return {
            tasks: null,
            EditTask: EditTask,
            statusTask: false,
        };
    },
    props: {
        task: Object,
        id: Number,
    },
    methods: {
        deleteTask(id) {
            try {
                let tasks = JSON.parse(appSettings.getString("todo"));
                tasks.splice(id, 1);
                appSettings.setString("todo", JSON.stringify(tasks));
                this.tasks = JSON.parse(appSettings.getString("todo"));
                this.$emit("update", { tasks: this.tasks });
            }
            catch (err) {
                console.log(err);
            }
        },
        complete(id) {
            let tasks = JSON.parse(appSettings.getString("todo"));
            tasks[id].statusTask = this.statusTask = !tasks[id].statusTask;
            appSettings.setString("todo", JSON.stringify(tasks));
            this.tasks = JSON.parse(appSettings.getString("todo"));
            this.$emit("update", { tasks: this.tasks });
        }
    },
}
</script>

<style src="../style.css"></style>