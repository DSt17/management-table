<template>
    <tbody>
    <tr>
	  <td class="user-box" :class="!toggleCheckedValue ? 'employeeBackgroundColor' : '' ">
		<div class="user-box-header">
		    <div style="width: 100%;">
			  {{ item.title }}
		    </div>
		    <div>
			  <span style="cursor: pointer" @click="showAddTaskPopup" class="material-icons">playlist_add</span>
		    </div>
		</div>
	  </td>
	  <td v-for="(day,idx) in arrayForRendering()"
		:data-key="day"
		:key="idx"
	  >
	  </td>
    </tr>
    <TasksRow
	    v-for="(project,idx) in item.projects"
	    :month="month"
	    :key="idx"
	    :monthsState="monthsState"
	    :project="project"
	    :item="item"
	    :rangeDayArray="rangeDayArray"
	    :clickedCalendar="clickedCalendarValue"
	    :toggleCheckedValue="toggleCheckedValue"
	    @showAddTaskCalendarPopupVisible="showAddTaskCalendarPopupVisible"
	    @showAddTaskOnClickUnderTheHading="showAddTaskOnClickUnderTheHading"
    />
    </tbody>
</template>


<script>
import TasksRow from '@/components/Table/TaskRow'

export default {
    props: ["item", "monthsState", "rangeDayArray", "clickedCalendarValue", "toggleCheckedValue", "month"],
    data() {
	  return {
		clicked: false,
		currentMonth: 'October',
	  }
    },
    methods: {
	  arrayForRendering() {
		if (this.clickedCalendarValue === true) {
		    return this.rangeDayArray
		} else {
		    this.month !== '' ? this.currentMonth = this.month : ''
		    return this.monthsState[this.currentMonth]
		}
	  },
	  showAddTaskCalendarPopupVisible(ArraySelectedDays, customer) {
		this.$emit('showAddTaskCalendarPopupVisible', this.item, ArraySelectedDays, customer)
	  },
	  showAddTaskOnClickUnderTheHading(ArraySelectedDays, customer) {
		this.$emit('showAddTaskOnClickUnderTheHading', this.item, ArraySelectedDays, customer)
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

.user-box {
    min-width: 150px;
    max-width: 150px;
    font-size: 13px;
    font-weight: bold;
    background-color: #346977;
    color: white;
    position: sticky;
    left: 0
}

.user-box-header {
    display: flex;
    flex-direction: row;
    justify-content: space-between;
    align-items: center
}

.employeeBackgroundColor {
    background-color: #666060
}
</style>