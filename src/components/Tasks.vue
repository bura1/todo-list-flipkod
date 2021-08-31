<template>
    <div>
        <div class="task-modal">
            <b-button v-b-modal.modal-prevent-closing>Dodaj novi task</b-button>
            <div class="task-modal">
                <b-modal
                    id="modal-prevent-closing"
                    ref="modal"
                    title="Dodaj novi task"
                    @show="resetModal"
                    @hidden="resetModal"
                    @ok="handleOk"
                >
                    <form ref="form" @submit.stop.prevent="handleSubmit">
                        <b-form-group
                            label="Name"
                            label-for="name-input"
                            invalid-feedback="Name is required"
                            :state="nameState"
                        >
                            <b-form-input
                                id="name-input"
                                v-model="taskName"
                                :state="nameState"
                                required
                            ></b-form-input>
                        </b-form-group>
                        <b-form-group
                            label="Description"
                            label-for="description-input"
                            invalid-feedback="Description is required"
                            :state="descriptionState"
                        >
                            <b-form-input
                                id="description-input"
                                v-model="taskDescription"
                                :state="descriptionState"
                                required
                            ></b-form-input>
                        </b-form-group>
                    </form>
                </b-modal>
            </div>
        </div>

        <b-table 
            id="my-table"
            hover 
            per-page="5" 
            :current-page="currentPage"
            :items="items"
            :fields="fields"
            :sort-by.sync="sortBy"
            :sort-desc.sync="sortDesc"
            responsive="sm"
        >
            <template #cell(delete)="data">
                <b-button @click="deleteTask(data.value)">Delete</b-button>
            </template>
        </b-table>

        <div>
            Sorting By: <b>{{ sortBy }}</b>, Direction:
            <b>{{ sortDesc ? 'Descending' : 'Ascending' }}</b>
        </div>

        <b-pagination
            v-model="currentPage"
            :total-rows="rows"
            :per-page="perPage"
            aria-controls="my-table"
        ></b-pagination>
    </div>
</template>

<script>
export default {
    name: 'Tasks',
    components: {
    },
    data() {
        return {
            taskName: '',
            taskDescription: '',
            nameState: null,
            descriptionState: null,
            perPage: 5,
            currentPage: 1,
            sortBy: 'date',
            sortDesc: false,
            searchText: '',
            fields: [
                { key: 'ID', sortable: true },
                { key: 'name', sortable: true },
                { key: 'description', sortable: true },
                { key: 'date', sortable: true },
                { key: 'delete', sortable: false }
            ],
            items: [
                { ID: 1, name: 'Task 1', description: 'Opis 1', date: '5.8.2021 16:48:23', delete: 1 },
                { ID: 2, name: 'Task 2', description: 'Opis 2', date: '6.8.2021 10:30:23', delete: 2 },
                { ID: 3, name: 'Task 3', description: 'Opis 3', date: '6.8.2021 11:40:23', delete: 3 },
                { ID: 4, name: 'Task 4', description: 'Opis 4', date: '8.8.2021 9:18:23', delete: 4 },
                { ID: 5, name: 'Task 5', description: 'Opis 5', date: '9.8.2021 12:12:23', delete: 5 },
                { ID: 6, name: 'Task 6', description: 'Opis 6', date: '10.8.2021 20:41:23', delete: 6 }
            ]
        }
    },
    computed: {
        rows() {
            return this.items.length
        }
    },
    methods: {
        deleteTask(id) {
            var newItems = [];
            for(var i = 0; i <= this.items.length - 1; i++){
                if(this.items[i]['ID'] != id){
                    newItems.push(this.items[i]);
                }
            }
            this.items = newItems;
        },
        checkFormValidity() {
            const valid = this.$refs.form.checkValidity();
            this.nameState = valid;
            this.descriptionState = valid;
            return valid;
        },
        resetModal() {
            this.taskName = '';
            this.taskDescription = '';
            this.nameState = null;
            this.descriptionState = null;
        },
        handleOk(bvModalEvt) {
            bvModalEvt.preventDefault();
            this.handleSubmit();
        },
        handleSubmit() {
            if (!this.checkFormValidity()) {
                return;
            }

            var d = new Date();
            d.setHours(d.getHours() + 2);
            var dateString = d.getUTCDate() +"."+ (d.getUTCMonth()+1) +"."+ d.getUTCFullYear() + " " + d.getUTCHours() + ":" + d.getUTCMinutes() + ":" + d.getUTCSeconds();
            var id = 1;
            for(var i = 0; i <= this.items.length - 1; i++){
                if (this.items[i]['ID'] > 0) {
                    id = this.items[i]['ID'] + 1;
                }
            }
            var valueToPush = {};
            valueToPush['ID'] = id;
            valueToPush['name'] = this.taskName;
            valueToPush['description'] = this.taskDescription;
            valueToPush['date'] = dateString;
            valueToPush['delete'] = id;
            this.items.push(valueToPush);

            this.$nextTick(() => {
                this.$bvModal.hide('modal-prevent-closing');
            })
        }
    }
}
</script>

<style scoped>
.search {
    margin: 25px auto;
    max-width: 400px;
}
.task-modal {
    margin-bottom: 20px;
}
</style>