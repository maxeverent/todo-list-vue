<template>
    <Page class="body-home">
        <ActionBar>
            <Label text="ToDo" class="header"/>
        </ActionBar>
        <StackLayout>
            <Button text="Back" @tap="$navigateTo(Home)" class="add-button" style="background-color: white;" />
            <StackLayout class="body-task-add" style="height: 1000px;">
                <Label v-model="task.title" class="task-title-add" />
                <TextView v-model="task.content" class="task-content-edit" editable="false" />
                <Button text="Delete" @tap="$navigateTo(Home, {props: {deleteTaskId}})" class="delete-button" /> 
            </StackLayout>
            <StackLayout class="body-task-edit" style="height: 1100px;" >
                <TextField v-model="editTask.title" class="task-title-add" hint="Title" />
                <TextView v-model="editTask.content" class="task-content-edit" hint="Content" />
                <Label v-if="fields" text="Fields are empty" class="text-error" />
                <Button text="Edit" @tap="editTasks" class="edit-button" />
            </StackLayout>
        </StackLayout>
    </Page>
</template>

<script>

import Home from './Home.vue'

    export default {
        data() {
            return {
                Home: Home,
                editTask: {
                    id: this.id,
                    title: '',
                    content: '',
                },
                fields: false,
                deleteTaskId: this.id
            }
        },
        props: {
            id: Number,
            task: Object,
        },
        methods: {
            editTasks() {
                if (this.editTask.title !== "" && this.editTask.content !== "") {
                    let editTask = this.editTask
                    this.$navigateTo(Home, {props: {editTask}})
                }
                else {
                    this.fields = true
                    this.editTask.title = this.editTask.content = ''               
                }                
            }
        }
    }
</script>

<style src="../style.css"></style>