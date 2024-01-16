<template>
  <div class="mx-40">


    <div class="flex flex-row items-center justify-between">
      <h2 class="text-5xl font-black text-gray-900"> Lists</h2>
      <Button icon-left="plus">New List</Button>
    </div>
    <div class="mt-2">
      <Card title="General">
        <div>
          <ul>
            <li class="flex flex-row space-y-2 items-center justify-between" v-for="actions in actions.data"
              :key="actions.title">
              <router-link :to="`/actions/${actions.title}`">{{ actions.title }}</router-link>
              <Button @click="completeAction(actions.title)" icon="check" />
            </li>
          </ul>
          <Button @click="addActionDialogShown = true" icon-left="plus">New Action</Button>
        </div>
      </Card>
    </div>
    <Dialog v-model="addActionDialogShown">
      <template #body-title>
        <h3>Add New Action</h3>
      </template>
      <template #body-content>
        <div class="space-y-2">
          <Input v-model="action.title" type="text" required label="Title" />
          <Input v-model="action.category" type="select" required label="list" :options="categoryOptions" />
          <Input v-model="action.date" type="date" required label="Date" />
          <Input v-model="action.due_date" type="date" required label="Due Date" />
          <Input v-model="action.status" type="select" required label="Status" :options="['ToDo','Completed','Archived']" />
        </div>
      </template>
      <template #actions>
        <Button variant="solid" @click="addAction()">
          Add Action
        </Button>
        <Button class="ml-2" @click="addActionDialogShown = false">
          Close
        </Button>
      </template>
    </Dialog> 



  </div>
</template>

<script setup>
import { Dialog, createListResource, Card, Input } from 'frappe-ui';
import { reactive, ref, computed } from 'vue';

const action = {
  title: '',
  category: 'Home',
  date:'',
  due_date:'',
  status:'ToDo'
 

}

const addActionDialogShown = ref(false)

const actions = createListResource({
  doctype: 'Action',
  fields: ["title", "status", "date", "due_date", "description", "tasks", "category"],
  filters: {
    status: 'ToDo',
  },
  limit: 100
})
actions.reload()

const categories = createListResource({
  doctype: 'Category',
  fields: ['name'],
  transform(categories) {
    return categories.map((c) => c.name)
  }
})
categories.reload()

const categoryOptions = computed(() => {
  if (categories.list.loading || !categories.data) {
    return []
  }
  return categories.data


})

const completeAction = (actionName) => {
  actions.setValue.submit({
    name: actionName,
    status: 'Completed',
    onSuccess() {
      actions.reload()
    }
  })
}
const addAction = () => {
  console.log(5)
  console.log(action)
  actions.insert.submit(action)
}
</script>
