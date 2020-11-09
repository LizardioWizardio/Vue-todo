<template>
   <div>
       <h1>List</h1>
       <div class="row">
            <div class="s6 col">
                <select ref="select" v-model="filter">
                    <option value="" disabled selected>Choose task status</option>
                    <option value="active">Active</option>
                    <option value="outdated">Outdated</option>
                    <option value="completed">Completed</option>
                </select>
                <label>Filter</label>
            </div>
       </div>
       <button v-if="this.filter" class="btn btn-small red" @click="filter = null">Clear filter</button>
      
       <hr>
       <table v-if="tasks.length">
           <thead>
               <tr>
                   <th>#</th>
                   <th>Title</th>
                   <th>Date</th>
                   <th>Description</th>
                   <th>Status</th>
                   <th>Open</th>
                   <th>Close</th>
               </tr>
           </thead>
           <tbody>
               <tr v-for="(task, index) of displayTasks" :key="task.id">
                   <td>{{index + 1}}</td>
                   <td>{{task.title}}</td>
                   <td>{{new Date(task.date).toLocaleDateString()}}</td>
                   <td class="description"><div class="text">{{task.description}}</div></td>
                   <td>{{task.status}}</td>
                   <td>
                       <router-link tag="button" class="btn btn-small" :to="'/task/' + task.id">Open</router-link>
                   </td>
                   <td><button class="btn btn-small red" @click="deleteTask(task.id)">Delete</button></td>
               </tr>
           </tbody>
       </table>
       <p v-else>No tasks!</p>
   </div>
</template>

<script>
export default {
    data: () => ({
        filter: null
    }),
    methods: {
        deleteTask(taskId) {
            this.$store.dispatch('deleteTask', taskId)
        }
    },
    computed: {
        tasks() {
            return this.$store.getters.tasks
        },
        displayTasks() {
            return this.tasks.filter(task => {
                if (!this.filter) {
                    return true
                }
                return task.status === this.filter
            })
        }
    },
    mounted() {
        M.FormSelect.init(this.$refs.select)
    }
}
</script>

<style lang="scss" scoped>
    .text {
        white-space: nowrap;
        text-overflow: ellipsis;
        overflow: hidden;
    }
    .description {
        max-width: 400px;
    }
</style>