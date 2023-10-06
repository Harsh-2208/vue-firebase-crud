

<template>
    <div class="h-[calc(100vh-120px)] bg-gray-100 p-[20px]">
        <div
            class="container-lg max-w-7xl mt-6 bg-white mx-auto  shadow-lg shadow-slate-300 border border-gray-300 rounded-md p-[40px] ">

            <div class="text-right m-5">

                <button
                    class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded-full focus:outline-none focus:shadow-outline"
                    type="button" @click="showAddTaskModal = true">
                    +
                </button>
            </div>
            <div class="">
                <table class="w-full text-xl text-left    border border-slate-200 border-2 ">
                    <thead class="bg-gray-600 text-white ">
                        <tr>
                            <th class="border border-slate-300 p-[10px]">Title</th>
                            <th class="border border-slate-300 p-[10px]">Description</th>
                            <th class="border border-slate-300  p-[10px]">Status</th>
                            <th class="border border-slate-300  p-[10px]">Action</th>
                        </tr>
                    </thead>
                    <tbody>
                        <tr v-for="(task, taskId) in taskList">
                            <td class="border border-slate-300 rounded-xl p-[10px]">{{ task.title }}</td>
                            <td class="border border-slate-300 rounded-xl p-[10px]">{{ task.description }}</td>
                            <td class="border border-slate-300 rounded-xl p-[10px]">{{ task.status }}</td>
                            <td class="border border-slate-300 rounded-xl p-[10px]">
                                <!-- Trigger button to open the delete modal -->
                                <button class="mr-4" @click="openDeleteModal(taskId)"><i class="fa fa-trash"
                                        aria-hidden="true"></i></button>
                                <button @click="openEditModal(taskId)"><i class="fa fa-pencil"
                                        aria-hidden="true"></i></button>
                            </td>

                        </tr>
                    </tbody>
                </table>
            </div>
            <AddTask v-if=showAddTaskModal @cancel="showAddTaskModal = false" @submitted="showAddTaskModal = false">
            </AddTask>
            <EditTask v-if=showEditTaskModal @cancel="showEditTaskModal = false" @submitted="showEditTaskModal = false"
                :edit-id="editId"></EditTask>

            <!-- Delete Modal -->
            <DeleteModal v-if="showDeleteModal" @cancel="showDeleteModal = false" @deleted="showDeleteModal = false"
                :delete-id="deleteId" />
        </div>
    </div>
</template>

<script >
// import ref from 'vue'
import AddTask from '../components/AddTask.vue'
import EditTask from '../components/EditTask.vue'
import DeleteModal from './DeleteModal.vue'; // Import the DeleteModal component
import { getDatabase, ref, onValue } from "firebase/database";
export default {
    name: 'MainList',
    components: {
        AddTask,
        EditTask,
        DeleteModal
    },


    data() {
        return {
            showDeleteModal: false,
            showAddTaskModal: false,
            showEditTaskModal: false,
            deleteId: 0,
            editId: 0,
            taskList: []
        }
    },
    async mounted() {
        const firebaseDB = getDatabase();

        const firebaseRef = ref(firebaseDB, 'tasks/');
        onValue(firebaseRef, (snapshot) => {
            const data = snapshot.val();
            this.taskList = data
        })
    },
    methods: {
        openDeleteModal(id) {
            this.deleteId = id
            this.showDeleteModal = true
        },
        openEditModal(id) {
            this.editId = id
            this.showEditTaskModal = true
        }
    }
}

</script>