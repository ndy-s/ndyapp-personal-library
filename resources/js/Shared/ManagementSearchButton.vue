<template>
    <div class="flex items-center my-4 gap-4">
        <h2 class="w-full text-white text-3xl font-bold border-l-4 border-green-400 pl-4">{{ title }}</h2>

        <button class="border border-green-400 flex items-center p-2.5 text-md font-semibold text-white text-center bg-gray-800 hover:bg-gray-900" title="Upload Data" @click="uploadDataModal()">
            <svg fill="currentColor" height="20" width="25" version="1.1" id="Capa_1" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" 
                viewBox="0 0 490.955 490.955" xml:space="preserve">
                <path id="XMLID_448_" d="M445.767,308.42l-53.374-76.49v-20.656v-11.366V97.241c0-6.669-2.604-12.94-7.318-17.645L312.787,7.301
                C308.073,2.588,301.796,0,295.149,0H77.597C54.161,0,35.103,19.066,35.103,42.494V425.68c0,23.427,19.059,42.494,42.494,42.494
                h159.307h39.714c1.902,2.54,3.915,5,6.232,7.205c10.033,9.593,23.547,15.576,38.501,15.576c26.935,0-1.247,0,34.363,0
                c14.936,0,28.483-5.982,38.517-15.576c11.693-11.159,17.348-25.825,17.348-40.29v-40.06c16.216-3.418,30.114-13.866,37.91-28.811
                C459.151,347.704,457.731,325.554,445.767,308.42z M170.095,414.872H87.422V53.302h175.681v46.752
                c0,16.655,13.547,30.209,30.209,30.209h46.76v66.377h-0.255v0.039c-17.685-0.415-35.529,7.285-46.934,23.46l-61.586,88.28
                c-11.965,17.134-13.387,39.284-3.722,57.799c7.795,14.945,21.692,25.393,37.91,28.811v19.842h-10.29H170.095z M410.316,345.771
                c-2.03,3.866-5.99,6.271-10.337,6.271h-0.016h-32.575v83.048c0,6.437-5.239,11.662-11.659,11.662h-0.017H321.35h-0.017
                c-6.423,0-11.662-5.225-11.662-11.662v-83.048h-32.574h-0.016c-4.346,0-8.308-2.405-10.336-6.271
                c-2.012-3.866-1.725-8.49,0.783-12.07l61.424-88.064c2.189-3.123,5.769-4.984,9.57-4.984h0.017c3.802,0,7.38,1.861,9.568,4.984
                l61.427,88.064C412.04,337.28,412.328,341.905,410.316,345.771z"/>
            </svg>
            <span>Upload</span>
        </button>
                    
        <div class="flex w-full border border-green-400">
            <button class="flex-shrink-0 inline-flex items-center py-1.5 px-2.5 text-md font-semibold text-white text-center border-r border-green-400 bg-gray-800 hover:bg-gray-900" @click="openCreateModal()" title="Create Data">
                <svg xmlns="http://www.w3.org/2000/svg" width="20" height="25" fill="currentColor" class="bi bi-plus-circle" viewBox="0 0 16 16"> <path d="M8 15A7 7 0 1 1 8 1a7 7 0 0 1 0 14zm0 1A8 8 0 1 0 8 0a8 8 0 0 0 0 16z"/>
                    <path d="M8 4a.5.5 0 0 1 .5.5v3h3a.5.5 0 0 1 0 1h-3v3a.5.5 0 0 1-1 0v-3h-3a.5.5 0 0 1 0-1h3v-3A.5.5 0 0 1 8 4z"/>
                </svg>
                &nbsp;
                <span>Create Data</span>
            </button>
            <div class="relative w-full">
                <input 
                    type="search" 
                    v-model="params.search" 
                    aria-label="Search" 
                    placeholder="Type to search..." 
                    class="block p-2.5 pr-8 w-full text-md text-white bg-gray-800 placeholder:text-gray-400"
                />
                <button
                    v-show="params.search != undefined && params.search != ''"
                    @click="params.search = ''"
                    class="absolute top-1 right-0 pr-2 text-2xl font-extrabold text-green-400 w-8 h-8 hover:text-green-500"
                    title="Clear"
                >
                    <b>&times;</b>
                </button>
            </div>
        </div>
    </div>
</template>

<script setup>
    import { watch } from 'vue';
    import { debounce } from 'lodash/function';
    import { router } from "@inertiajs/vue3";
    
    const props = defineProps({
        title: String,
        params: Object,
        ArtLibrary: Object,
        uploadDataModal: Function,
        openCreateModal: Function,
    });

    const params = props.params;

    const handler = debounce(() => {
        const filterObject = (obj) => {
            const filtered = {};
            for (const key in obj) {
                if (obj[key] !== undefined && obj[key] !== null) {
                    filtered[key] = obj[key];
                }
            }
            return filtered;
        };

        const filteredParams = filterObject(params);
        router.get(props.ArtLibrary.first_page_url, filteredParams, {
            replace: true,
            preserveState: true,
            preserveScroll: true,
        });
    }, 150);

    watch(() => params, handler, { deep: true });
</script>