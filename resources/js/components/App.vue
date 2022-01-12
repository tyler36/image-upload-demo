<template>
    <div>
        <div class="mt-4">
            <file-pond
                name="image"
                ref="pond"
                label-idle="Click or drag image here"
                @init="filepondInitialized"
                @processfile="handleProcessedFile"
                accepted-file-types="image/*"
            />
        </div>

        <div class="mt-8 mb-24">
            <h3 class="text-2xl font-medium text-center">Image Gallery</h3>
            <div class="grid gird-cols-3 gap-2 justify-evenly mt-4">
                <div v-for="(image, index) in images" :key="index">
                    <img :src="'/storage/images/' + image ">
                </div>
            </div>
        </div>
    </div>
</template>


<script>
// Import FilePond
import vueFilePond, {setOptions} from 'vue-filepond';
import FilePondPluginFileValidateType from 'filepond-plugin-file-validate-type';
import "filepond/dist/filepond.min.css";

setOptions({
    server: {
        process: {
            url: '/upload',
            headers: {
                'X-CSRF-TOKEN': document.head.querySelector('meta[name="csrf_token"]').content
            }
        }
    }
});

const FilePond = vueFilePond(FilePondPluginFileValidateType);

export default {
    components: {
        FilePond
    },

    data() {
        return {
            images: []
        };
    },

    mounted() {
        axios.get('/images')
            .then((response) => this.images = response.data)
            .catch((error) => console.error(error));
    },

    methods: {
        filepondInitialized() {
            console.log('File pond is ready ...');
            console.log('Filepond object: ', this.$refs.pond);
        },

        handleProcessedFile(error, file) {
            if(error) {
                return console.error(error);
            }

            this.images.unshift(file.serverId);
        }
    }
};
</script>
