<template>
    <div class="h-100vh fixed inset-0 flex items-center justify-center z-50 bg-black/50 p-[40px]">
        <div class=" w-[80%] mt-6 bg-white mx-auto  shadow-lg shadow-slate-800   rounded-md p-[40px] ">
            <span class="text-2xl mb-8 bold">Add Task</span>
            <form class=" mx-auto p-4">
                <div class="mb-4">
                    <label class="block text-gray-700 text-sm font-bold mb-2" for="title">Title</label>
                    <input
                        class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline"
                        id="title" type="text" v-model="title" />
                </div>
                <div class="mb-4">
                    <label class="block text-gray-700 text-sm font-bold mb-2" for="description">Description</label>
                    <input
                        class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline"
                        id="description" type="text" v-model="description" />
                </div>
                <div>
                    <label class="block text-gray-700 text-sm font-bold mb-2" for="selectedOption">
                        Status
                    </label>
                    <select
                        class="block appearance-none w-full bg-white border border-gray-300 hover:border-gray-500 px-4 py-2 pr-8 rounded shadow leading-tight focus:outline-none focus:shadow-outline"
                        id="selectedOption" v-model="status">
                        <option value="" disabled>Select an option</option>
                        <option value="Complete">Complete</option>
                        <option value="Panding">Panding</option>
                        <option value="Review">Review</option>
                        <option value="Other">Other</option>
                    </select>
                </div>
                <div class="mb-6 mt-6 text-center">
                    <button
                        class="bg-blue-500 mr-4 hover:bg-blue-700 text-white mt-5 font-bold py-2 px-4 rounded focus:outline-none focus:shadow-outline"
                        type="button" @click="submitForm">
                        Submit
                    </button>
                    <button
                        class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded focus:outline-none focus:shadow-outline"
                        type="button" @click="cancelForm">
                        Cancel
                    </button>
                </div>
            </form>
        </div>

    </div>
</template>
  
<script>

import { getDatabase, ref, set } from "firebase/database";

export default {
    data() {
        return {
            title: '',
            description: '',
            status: '',
        };
    },
    methods: {
        cancelForm() {
            this.$emit('cancel');
        },
        generateSlug(title) {

            return title.toLowerCase().replace(/ /g, '-')
                .replace(/[^\w-]+/g, '');
        },
        submitForm() {
            const firebaseDB = getDatabase();
            const taskId = this.generateSlug(this.title);

            const firebaseRef = ref(firebaseDB, 'tasks/' + taskId);
            const data = {
                title: this.title,
                description: this.description,
                status: this.status,
            }
            set(firebaseRef, data)

            this.title = ''
            this.description = ''
            this.status = ''
            this.$emit('submitted');
        },

    },
};
</script>