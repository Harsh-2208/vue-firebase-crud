<template>
    <div class="fixed inset-0 flex items-center justify-center bg-black/50 z-50 ">
        <div class="bg-white p-6 rounded-lg shadow-lg">
            <h2 class="text-xl font-semibold mb-4">Confirm Deletion</h2>
            <p class="mb-4">Are you sure you want to delete this item?</p>
            <div class="flex justify-end">
                <button @click="cancelDelete"
                    class="px-4 py-2 mr-2 text-gray-600 hover:text-gray-800 hover:bg-gray-100 rounded">
                    Cancel
                </button>
                <button @click="confirmDelete" class="px-4 py-2 text-red-600 hover:text-red-800 hover:bg-red-100 rounded">
                    Delete
                </button>
            </div>
        </div>
    </div>
</template>
  
<script>
import { getDatabase, ref, remove } from "firebase/database";
export default {
    methods: {
        cancelDelete() {
            this.$emit('cancel');
        },
        confirmDelete() {
            const firebaseDB = getDatabase();
            const firebaseRef = ref(firebaseDB, 'tasks/' + this.deleteId);
            remove(firebaseRef)
            
            this.$emit('deleted');
        },
    },
    props: {
        deleteId: String
    }
};
</script>









