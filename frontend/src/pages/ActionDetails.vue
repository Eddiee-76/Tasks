<template>
    <div class="my-4 mx-20" v-if="!action.get.loading">
        <div class="flex flex-row items-center justify-between">
            <h1 class="font-black text-5xl text-gray-900">{{ action.doc.title }}</h1>
            <div class="flex flex-row items-center space-x-2">
                <Button icon-left="arrow-left" @click="router.back()">Go Back</Button>
                <Button @click="action.setValue.submit({ status: 'Archived' })" appearance="white"
                    class="text-red-800 border-red-900" icon-left="trash"
                    v-if="action.doc.status != 'Archived' && action.doc.status != 'Completed'">Delete</Button>
                <Button @click="action.setValue.submit({ status: 'Completed' })" appearance="white"
                    class="text-green-800 border-green-900" icon-left="check"
                    v-if="action.doc.status != 'Completed' && action.doc.status != 'Archived'">Mark as Done</Button>
            </div>
        </div>
        <div>
            <!-- <TextEditor
    editor-class="prose-sm border max-w-none rounded-b-lg p-3 overflow-auto h-64 focus:outline-none"
    :fixedMenu="true"
    :content="content"
    @change="(val) => (content = val)"
                /> -->
        </div>
    </div>
    <LoadingIndicator v-else class="mx-80 my-10 w-10 text-blue-900" />
</template>
<script setup>
import { ref } from 'vue';
import { useRouter } from 'vue-router';
import { createDocumentResource, LoadingIndicator, TextEditor } from 'frappe-ui';
const action = createDocumentResource({
    doctype: 'Action',
    name: props.name,
})
action.reload()
const router = useRouter()
const props = defineProps(['name'])
const content = ref('')
</script>