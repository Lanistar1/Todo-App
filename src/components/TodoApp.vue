<template>
  <div class="container mx-auto">
      <h1 class="text-2xl -mt-6 font-bold  text-center ">My Weekly Todo App</h1>

      <!-- input field -->
    <div class=" justify-center mt-4 flex flex-row">
        <input type="text" class="mt-1 block h-10 w-3/6 focus:shadow-md border md:w-1/3 p-4" required autofocus placeholder="  Enter Task" v-model="task" />

        <a @click.stop="submitTask" class="rounded-r-md py-2 px-6 cursor-pointer tracking-wider text-md flex w-1/3 md:w-32 items-center
                text-center bg-purple-600 text-white font-bold hover:bg-purple-600 hover:shadow-purple mt-1 ">
                Submit</a>

    </div>
    <div>
        <button class="bg-gray-400 py-1 px-3 text-white rounded-md mt-2" @click="sortTask">Sort Priority</button>
    </div>

    <!-- Task table -->
    <div class="container flex justify-center mx-auto mt-6 ">
    <div class="flex flex-col overflow-x-auto sm:-mx-6 lg:-mx-8">
        <div class="w-full">
            <div class="border-b  border-gray-200 shadow">
                <table class="divide-y divide-gray-300 ">
                    <thead class="bg-purple-700">
                        <tr class="bg-purple-700">
                            <th class="px-6 py-3 text-md text-white">
                                Task
                            </th>
                            <th class="px-6 py-2 text-md text-white">
                                Status
                            </th>
                            <th class="px-6 py-2 text-md text-white">
                                Deadline
                            </th>
                            <th class="px-6 py-2 text-md text-white">
                                Priority
                            </th>
                            <th class="px-6 py-2 text-md text-white">
                                Edit
                            </th>
                            <th class="px-6 py-2 text-md text-white">
                                Delete
                            </th>
                        </tr>
                    </thead>
                    <tbody class="bg-white divide-y divide-gray-300">
                        <tr class="whitespace-nowrap" v-for="(task, index) in tasks" :key="index">
                            <td class="px-6 py-4">
                                <div class="text-sm text-gray-900">
                                    {{task.name}}
                                </div>
                            </td>
                            <td style="width: 120px" class="px-6 py-4">
                                <div class="text-sm text-gray-500"><span @click="changeStatus(index)" class="cursor-pointer">{{task.status}}</span></div>
                            </td>
                            <td style="width: 120px" class="px-6 py-4 text-sm text-gray-500">
                                <span @click="changeDeadline(index)" class="cursor-pointer">{{task.deadline}}</span>
                            </td>
                            <td style="width: 120px" class="px-6 py-4 text-sm text-gray-500">
                                <span @click="changePriority(index)" class="cursor-pointer">{{task.priority}}</span>
                            </td>
                            <td class="px-6 py-4">
                                <a href="#" @click="editTask(index)">
                                    <svg xmlns="http://www.w3.org/2000/svg" class="w-6 h-6 text-blue-400" fill="none"
                                        viewBox="0 0 24 24" stroke="currentColor">
                                        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                                            d="M11 5H6a2 2 0 00-2 2v11a2 2 0 002 2h11a2 2 0 002-2v-5m-1.414-9.414a2 2 0 112.828 2.828L11.828 15H9v-2.828l8.586-8.586z" />
                                    </svg>
                                </a>
                            </td>
                            <td class="px-6 py-4">
                                <a href="#" @click="deleteTask(index)">
                                    <svg xmlns="http://www.w3.org/2000/svg" class="w-6 h-6 text-red-400" fill="none"
                                        viewBox="0 0 24 24" stroke="currentColor">
                                        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                                            d="M19 7l-.867 12.142A2 2 0 0116.138 21H7.862a2 2 0 01-1.995-1.858L5 7m5 4v6m4-6v6m1-10V4a1 1 0 00-1-1h-4a1 1 0 00-1 1v3M4 7h16" />
                                    </svg>
                                </a>
                            </td>
                        </tr>
                    </tbody>
                </table>
            </div>
        </div>
    </div>
</div>
  </div>
</template>

<script>
export default {
    name: 'HelloWorld',
    props: {
        msg: String,
    },

    data() {
        return {
            task: '',
            editedTask: null,
            availableStatuses: ['to-do', 'in-progress', 'finished'],
            availableDeadlines: ['monday', 'tuesday', 'wednesday', 'thursday', 'friday', 'saturday', 'sunday'],
            availablePriorities: [1, 2, 3],

            tasks: []
        }
    },

    methods: {
        submitTask() {
            if(this.task === '') return;
            alert(this.task)

            if(this.editedTask === null){
                this.tasks.push({
                    name: this.task,
                    status: 'to-do',
                    deadline: 'monday',
                    priority: 3
                });
            }else {
                this.tasks[this.editedTask].name = this.task;
                this.editedTask = null;
            }

            this.task = '';
        },

        deleteTask(index){
            this.tasks.splice(index, 1);
        },  

        editTask(index){
            this.task = this.tasks[index].name;
            this.editedTask = index;
        },

        changeStatus(index){
            let newIndex = this.availableStatuses.indexOf(this.tasks[index].status);
            if(++newIndex > 2) newIndex = 0;
            this.tasks[index].status = this.availableStatuses[newIndex];
        },
        changeDeadline(index){
            let newIndex = this.availableDeadlines.indexOf(this.tasks[index].deadline);
            if(++newIndex > 6) newIndex = 0;
            this.tasks[index].deadline = this.availableDeadlines[newIndex];
        },
        changePriority(index){
            let newIndex = this.availablePriorities.indexOf(this.tasks[index].priority);
            if(++newIndex > 2) newIndex = 0;
            this.tasks[index].priority = this.availablePriorities[newIndex];
        },
        sortTask(){
            alert("sorting")
            this.tasks = this.tasks.sort((a, b) => b.priority-a.priority)
        }
    }
};
</script>

<style>

</style>