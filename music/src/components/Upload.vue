<template>
    <div class="bg-white rounded border border-gray-200 relative flex flex-col">
        <div class="px-6 pt-6 pb-5 font-bold border-b border-gray-200">
            <span class="card-title">Upload</span>
            <i class="fas fa-upload float-right text-green-400 text-2xl"></i>
        </div>
        <div class="p-6">
            <!-- Upload Dropbox -->
            <div
                class="w-full px-10 py-20 rounded text-center cursor-pointer border border-dashed
                border-gray-400 text-gray-400 transition duration-500 hover:text-white
                hover:bg-green-400 hover:border-green-400 hover:border-solid"
                :class="{'bg-green-500 border-green-400 border-solid':is_drageover}"
                @drag.prevent.stop=""
                @dragstart.prevent.stop=""
                @dragend.prevent.stop="is_drageover = false"
                @dragleave.prevent.stop="is_drageover = false"
                @dragover.prevent.stop="is_drageover = true"
                @dragenter.prevent.stop="is_drageover = true"
                @drop.prevent.stop="upload"
            >
                <h5>Drop your files here</h5>
            </div>
            <hr class="my-6" />
            <!-- Progess Bars -->
            <div class="mb-4" v-for="upload in uploads" :key="upload.name">
                <!-- File Name -->
                <div class="font-bold text-sm">{{ upload.name }}</div>
                <div class="flex h-4 overflow-hidden bg-gray-200 rounded">
                    <!-- Inner Progress Bar -->
                    <div class="transition-all progress-bar bg-blue-400"
                         :class="'bg-blue-400'"
                         :style="{width: upload.current_progress + '%'}"
                    ></div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>

import { ref, uploadBytesResumable } from "firebase/storage";
import { storage } from "@/includes/firebase";

export default {
    name: "AppUpload",
    data() {
        return {
            is_drageover: false,
            uploads: []
        };
    },
    methods: {
        upload(event) {
            this.is_drageover = false;
            const files = [...event.dataTransfer.files];
            files.forEach(file => {
                if (file.type !== "audio/mpeg") {
                    return;
                }

                const songsRef = ref(storage, `songs/${file.name}`);
                const uploadTask = uploadBytesResumable(songsRef, file);
                const uploadIndex = this.uploads.push({
                    task: uploadTask,
                    current_progress: 0,
                    name: file.name
                }) - 1;
                uploadTask.on("state_changed", snapshot => {
                    const progress = (snapshot.bytesTransferred / snapshot.totalBytes) * 100;
                    this.uploads[uploadIndex].current_progress = progress;
                });

            });

        }
    }
};
</script>

<style scoped>

</style>