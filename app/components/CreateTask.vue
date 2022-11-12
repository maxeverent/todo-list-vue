<template>
    <Page class="body-home">
        <ActionBar>
            <Label text="ToDo" class="header"/>
        </ActionBar>
        <StackLayout class="body-task-add">
            <Button text="Back" @tap="$navigateTo(Home)" class="add-button" style="background-color: white;" />
            <TextField v-model="task.title" class="task-title-add" hint="Title" />
            <TextView v-model="task.content" class="task-content-add" hint="Content" />
            <Label v-if="fields" text="Fields are empty" class="text-error" />
            <Button text="Add" @tap="goToHome" class="add-button" style="background-color: white;" />
        </StackLayout>
    </Page>
</template>

<script>
import home from './Home.vue'
    export default {
        data() {
            return {
                Home: home,
                task: {
                    title: '',
                    content: '',
                    statusTask: false,
                },
                fields: false
            }
        },
        props: {
            
        },
        methods: {
            goToHome() {
                if (this.task.title !== "" && this.task.content !== "") {
                    let task = this.task
                    this.$navigateTo(home, {props: {task}})
                }
                else {
                    this.fields = true
                    this.task.title = this.task.content = ''               
                }
            }
        },
    }
</script>

<style src="../style.css"></style>