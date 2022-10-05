<template>
    <tbody>
    <tr>
	  <td class="user-box-header">
		<div style="display: flex; flex-direction: row; justify-content:space-between;align-items: center">
		    <div style="width: 125px">
			  {{ item.title }}
		    </div>
		    <div>
			  <span style="cursor: pointer" @click="showAddTaskPopup" class="material-icons">playlist_add</span>
		    </div>
		</div>
	  </td>
	  <td v-for="(i,idx) in arrayForRendering()"
		:key="idx">
	  </td>
    </tr>
    <TasksRow
	    v-for="(item,idx) in item.projects"
	    :key="idx"
	    :dayInAMonths="dayInAMonths"
	    :item="item"
	    :rangeDayArray="rangeDayArray"
	    :clickedCalendar="clickedCalendarValue"
    />
    </tbody>
</template>


<script>
import TasksRow from '@/components/Table/TaskRow'

export default {
    props: ["item", "dayInAMonths", "rangeDayArray", "clickedCalendarValue"],
    data() {
	  return {
		clicked: false,
	  }
    },
    methods: {
	  arrayForRendering() {
		if (this.clickedCalendarValue === true) {
		    return this.rangeDayArray
		} else {
		    return this.dayInAMonths
		}
	  },
	  showAddTaskPopup() {
		this.$emit('showAddTaskPopup', this.item)
	  }
    },
    components: {TasksRow},
}
</script>


<style>
td {
    border: 1px solid black;
    border-top: none;
}

.user-box-header {
    min-width: 150px;
    max-width: 150px;
    font-size: 13px;
    font-weight: bold;
    background-color: #346977;
    color: white;
}
</style>