<template>
    <div class="mt-4">
        <file-pond
            name="image"
            ref="pond"
            label-idle="Click or drag image here"
            @init="filepondInitialized"
            accepted-file-types="image/*"
        />
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
        return {};
    },

    methods: {
        filepondInitialized() {
            console.log('File pond is ready ...');
            console.log('Filepond object: ', this.$refs.pond);
        }
    }
};
</script>
