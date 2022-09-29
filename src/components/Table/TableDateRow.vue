<template>
    <tbody>
    <tr>
	  <td class="user-box-header">
		<div style="display: flex; flex-direction: row; justify-content:center;align-items: center">
		    <div style="width: 100px">
			  {{ item.title }}
		    </div>
		    <div class="dropdown-box">
			  <i class="material-icons menu">add_task</i>
			  <div class="dropdown-content">
				<div>
				    <button>Add Task</button>
				</div>
				<div>
				    <button> Settings</button>
				</div>
			  </div>
		    </div>
		</div>
	  </td>
	  <td v-for="(dayInAMonths,idx) in dayInAMonths"
		v-bind:key="idx">
	  </td>
    </tr>
    <TasksRow
	    v-for="(item,idx) in item.projects"
	    :key="idx"
	    :dayInAMonths="dayInAMonths"
	    :item="item"
    />
    </tbody>
</template>


<script>
import TasksRow from '@/components/Table/TaskRow'


export default {
    props: ["item", "dayInAMonths"],
    data() {
	  return {
		clicked: false,
	  }
    },
    methods: {},
    components: {TasksRow},
}
</script>


<style>
td {
    border: 1px solid black;
    border-top: none;
    width: 25px;
    height: 25px;
}

.dropdown-box {
    position: relative;
    display: inline-block;
}

.dropdown-content {
    display: none;
    position: absolute;
    background-color: #f1f1f1;
    min-width: 160px;
    box-shadow: 0 8px 16px 0 rgba(0, 0, 0, 0.2);
    z-index: 1;
}

.dropdown-content button {
    width: 100%;
    height: 100%;
}

.dropdown-content a:hover {
    background-color: #ddd;
}

.dropdown-box:hover .dropdown-content {
    display: block;
}

.user-box-header {
    width: 120px;
    font-size: 13px;
    font-weight: bold;
    background-color: #346977;
    color: white
}

</style>