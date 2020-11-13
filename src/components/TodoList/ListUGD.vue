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
                <template v-slot:[`item.actions`]="{ item }">
                    <v-btn small class="mr-2" @click="editItem(item)">
                        edit
                    </v-btn>
                    <v-btn small @click="deleteItem(item)">
                        delete
                    </v-btn>
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

        <v-row justify="center">
            <v-dialog v-model="dialogConfirm" persistent max-width="290">
            <v-card>
                <v-card-title class="error headline" style="font-weight:bold; color:white;">
                Confirm Delete Data
                </v-card-title>
                <v-card-text>Anda yakin ingin menghapus data?</v-card-text>
                <v-card-actions>
                <v-spacer></v-spacer>
                <v-btn color="green darken-1" text @click="dialogConfirm = false">
                    Tidak
                </v-btn>
                <v-btn color="green darken-1" text @click="dialogDelete(item)">
                    Ya
                </v-btn>
                </v-card-actions>
            </v-card>
            </v-dialog>
        </v-row>
        
    </v-main>
</template>
<script>
export default {
name: "List",
data() {
    return {
        search: null,
        dialog: false,
        dialogConfirm: false,
        editID: null,
        deleteID: null,
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
    };
},
methods: {
    save() {
        if(this.editID == null)
        {
            this.todos.push(this.formTodo);
        } else{
            this.todos[this.editID].task = this.formTodo.task
            this.todos[this.editID].priority = this.formTodo.priority
            this.todos[this.editID].note = this.formTodo.note
        }
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
        this.editID = null,
        this.deleteID = null
    },
    editItem(item) {
        this.dialog = true;
        this.editID = this.todos.indexOf(item);//mencari index 
        this.formTodo.task = this.todos[this.editID].task
        this.formTodo.priority = this.todos[this.editID].priority
        this.formTodo.note = this.todos[this.editID].note
    },
    deleteItem(item) {
        //this.todos.splice(this.todos.indexOf(item), 1)
        this.dialogConfirm = true;
        
    },
    dialogDelete(item) {
        this.todos.splice(this.todos.indexOf(item), 1)
        this.dialogConfirm = false;
    }
},
};
</script>


