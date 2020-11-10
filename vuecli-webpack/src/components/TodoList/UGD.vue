<template>
    <v-main class="list">
        <h3 class="text-h3 font-weight-medium mb-5">To Do List</h3>
            <v-card>
                <v-card-title>
                    <v-text-field
                        v-model="search"
                        append-icon="mdi-magnify"
                        label="Search"
                        single-line
                        hide-details
                    ></v-text-field>
                    <v-spacer></v-spacer>
                    <v-btn color="success" dark @click="dialog = true">
                        Tambah
                    </v-btn>
                </v-card-title>
                <v-data-table :headers="headers" :items="todos" :search="search">
                    <template v-slot:[`item.priority`]>
                        <v-chip v-if="formTodo.priority == 'Penting'" color="red" outlined label>{{ item.priority }}</v-chip>
                        <v-chip v-if="formTodo.priority == 'Biasa'" color="blue" outlined label>{{ item.priority }}</v-chip>
                        <v-chip v-if="formTodo.priority == 'Tidak penting'" color="green" outlined label>{{ item.priority }}</v-chip>
                    </template>
                    <template v-slot:[`item.actions`]="{ item }">
                        <v-icon small color="purple" class="mr-2" @click="detailItem(item)">
                            {{ icons.mdiTextBoxSearchOutline}}
                        </v-icon>
                        <v-icon small class="mr-2" color="blue" @click="editItem(item)">
                            {{ icons.mdiPencil }}
                        </v-icon>
                        <v-icon small class="mr-2" color="red" @click="deleteItem(item)">
                            {{ icons.mdiDelete }}
                        </v-icon>
                    </template>
                </v-data-table>
                </v-card>
                <v-dialog v-model="dialog" persistent max-width="600px">
                    <v-card>
                        <v-card-title>
                            <span class="headline">Form Todo</span>
                        </v-card-title>
                    <v-card-text>
                        <v-container>
                            <v-text-field
                                v-model="formTodo.task"
                                label="Task"
                                required
                            ></v-text-field>
                            <v-select
                                v-model="formTodo.priority"
                                :items="['Penting', 'Biasa', 'Tidak penting']"
                                label="Priority"
                                required
                            ></v-select>
                            <v-textarea
                                v-model="formTodo.note"
                                label="Note"
                                required
                            ></v-textarea>
                        </v-container>
                    </v-card-text>
                    <v-card-actions>
                        <v-spacer></v-spacer>
                        <v-btn color="blue darken-1" text @click="cancel">
                            Cancel
                        </v-btn>
                        <v-btn color="blue darken-1" text @click="save">
                            Save
                        </v-btn>
                </v-card-actions>
            </v-card>
        </v-dialog>
    </v-main>
</template>
<script>
import {
    mdiPencil,
    mdiDelete,
    mdiTextBoxSearchOutline,
} from '@mdi/js'

export default {
    name: "UGD",
    data() {
        return {
            search: null,
            dialog: false,
            headers: [
                {
                    text: "Task",
                    align: "start",
                    sortable: true,
                    value: "task",
                },
                { text: "Priority", value: "priority" },
                { text: "Note", value: "note" },
                { text: "Actions", value: "actions" },
            ],
            todos: [
                {
                    task: "bernafas",
                    priority: "Penting",
                    note: "huffttt",
                },
                {
                    task: "nongkrong",
                    priority: "Tidak penting",
                    note: "bersama tman2",
                },
                {
                    task: "masak",
                    priority: "Biasa",
                    note: "masak air 500ml",
                },
            ],
            formTodo: {
                task: null,
                priority: null,
                note: null,
            },
            detail: {
                task: null,
                priority: null,
                note: null,
            }
        };
    },
    methods: {
        save() {
            this.todos.push(this.formTodo);
            this.resetForm();
            this.dialog = false;
        },
        cancel() {
            this.resetForm();
            this.dialog = false;
        },
        resetForm() {
            this.formTodo = {
                task: null,
                priority: null,
                note: null,
            };
        },
        deleteItem(item){
            this.todos = this.todos.filter(todo => todo !== item);
        },
        detailItem(item) {
            this.detail = {
                task: item.task,
                priority: item.priority,
                note: item.note,
            }
            this.dialognote = true;
        },
        editItem(item){
            this.formTodo = {
                task: item.task,
                priority: item.priority,
                note:  item.note,
            }
            this.dialog = true;
            this.deleteItem(item)
        },
        updateItem(){
            this.save();
        },
    },
};
</script>