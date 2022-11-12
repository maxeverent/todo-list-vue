<template>
    <Page class="body-home">
        <ActionBar>
            <Label text="ToDo" class="header" />
        </ActionBar>
        <StackLayout>
            <Button text="Add" @tap="$navigateTo(CreateTask)" class="add-button" />
            <StackLayout v-if="allTasks">
                <FlexboxLayout>
                    <Button v-if="completedTasksOrNot" text="Completed" @tap="completed" class="sort-button" style="color: white" />
                    <Button v-else text="In work" @tap="inWork" class="sort-button" style="color: white" />
                    <Button v-if="completedTasksOrNot === false" text="Completed" @tap="completed" class="sort-button" />
                    <Button v-else text="In work" @tap="inWork" class="sort-button" />
                </FlexboxLayout>
                <template >
                    <ScrollView orientation="vertical" height="2000px">
                        <StackLayout orientation="vertical">
                            <Task v-for="task, index in tasks" 
                                :task="task" 
                                :key="index" 
                                :id="index" 
                                @update="update" 
                                v-if="tasks[index].statusTask === completedTasksOrNot" 
                            />
                        </StackLayout>
                    </ScrollView>
                </template> 
            </StackLayout>
            <StackLayout v-else>
                <Label text="No tasks" class="no-tasks" />
            </StackLayout>
        </StackLayout>
    </Page>
</template>

<script>
import Task from './Task.vue'
import createTask from '../components/CreateTask.vue'

const appSettings = require('@nativescript/core/application-settings');

  export default {
    data() {
        return {
            CreateTask: createTask,
            tasks: JSON.parse(appSettings.getString("todo")),
            allTasks: true,
            completedTasksOrNot: false,
        }
    },
    props: {
        task: Object,
        editTask: Object,
        deleteTaskId: Number,
    },
    methods: {
        update(data) {
            this.tasks = data.tasks
        },
        completed() {
            this.completedTasksOrNot = true
        },
        inWork() {
            this.completedTasksOrNot = false
        },
    },
    components: { Task },
    watch: {
        tasks() {
            if (this.tasks.length == 0 ) {
                this.allTasks = false
            }
        },
    },
    mounted() {
        if (this.tasks.length == 0 ) {
            this.allTasks = false
        }
        if (this.deleteTaskId >= 0) {
            try {
                console.log(this.deleteTaskId)
                let tasks = JSON.parse(appSettings.getString("todo"))
                tasks.splice(this.deleteTaskId, 1)
                console.log(tasks)
                appSettings.setString("todo", JSON.stringify(tasks))
                this.tasks = JSON.parse(appSettings.getString("todo"))
            }
            catch (err) {
                console.log(err)
            }
        }
        if (!!this.task) {
            this.tasks.push(this.task)  
            appSettings.setString("todo", JSON.stringify(this.tasks)) 
            if (this.tasks.length !== 0 ) {
                this.allTasks = true
            }        
        }
        if (!!this.editTask) {
            let tasks = JSON.parse(appSettings.getString("todo"))
            tasks[this.editTask.id].title = this.editTask.title
            tasks[this.editTask.id].content = this.editTask.content
            appSettings.setString("todo", JSON.stringify(tasks))
            this.tasks = JSON.parse(appSettings.getString("todo"))  
        }
    }
  };
</script>

<style src="../style.css"></style>
