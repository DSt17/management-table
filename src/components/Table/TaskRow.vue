<template>
    <tr>
	  <td style=" font-size: 10px; position: sticky; left: 0; background-color: white; width: 150px; cursor: pointer"
		@click="openOnClickUnderTheHading"
	  >
		{{ project.task }} ({{ project.customer }})
	  
	  </td>
	  <td v-for="(day,idx) in arrayForRendering()"
		:data-key="day"
		:key="idx"
		:class="selected.includes(key(day)) === true ?  'selected' : workAndWeekendClass(day)"
		@mousedown="select"
		@mouseover="select"
		@mouseup="openPopup"
	  >
		<span style="font-size: 10px">
		    {{ Date.parse(day) === Date.parse(project.dayStart) ? project.workingTimeHours : '' }}
		</span>
	  </td>
    </tr>
</template>

<script>

export default {
    name: "TaskRow",
    props: ["project", "item", "monthsState", "rangeDayArray", "clickedCalendar", "toggleCheckedValue", "month"],
    data: () => ({
	  selected: [],
	  addTaskCalendarPopupVisible: false,
	  toggleChecked: '',
	  currentMonth: 'October',
    }),
    methods: {
	  key: (day) => `${day}`,
	  select({buttons, target: {dataset: {key}}}) {
		if (buttons) {
		    const index = this.selected.indexOf(key);
		    if (index !== -1) {
			  this.selected.splice(index, 1);
		    } else {
			  this.selected.push(key);
		    }
		}
	  },
	  openOnClickUnderTheHading() {
		if (!this.toggleChecked) {
		    this.$emit('showAddTaskOnClickUnderTheHading', this.project.customer)
		}
	  },
	  openPopup() {
		this.toggleChecked = this.toggleCheckedValue
		if (this.selected[this.selected.length - 1]) {
		    this.addTaskCalendarPopupVisible = true
		    if (!this.toggleChecked) {
			  this.$emit('showAddTaskCalendarPopupVisible', this.selected, this.project.customer)
		    }
		    this.selected = []
		}
	  },
	  arrayForRendering() {
		if (this.clickedCalendar === true) {
		    return this.rangeDayArray
		} else {
		    this.month !== '' ? this.currentMonth = this.month : ''
		    return this.monthsState[this.currentMonth]
		}
	  },
	  workAndWeekendClass(day) {
		let weekendDayCount = 0
		let dayStartParse = Date.parse(this.project.dayStart) / 86400000
		let dayEndParse = Date.parse(this.project.dayFinish) / 86400000
		let currentDay = Date.parse(day) / 86400000
		if (new Date(day).getDay() === 6 || new Date(day).getDay() === 0) {
		    return 'weekendDay'
		}
		if (this.item.vacation.find(el => el === day)) {
		    return 'vacationDay'
		}
		if (this.item.sickDay.find(el => el === day) === day) {
		    return 'sickDay'
		}
		if (currentDay >= dayStartParse && currentDay <= dayEndParse + weekendDayCount) {
		    if (new Date(day).getDay() === 6) {
			  weekendDayCount += 1
			  return 'weekendDay'
		    }
		    if (new Date(day).getDay() === 0) {
			  weekendDayCount += 1
			  return 'weekendDay'
		    } else {
			  let findetDate = this.arrayForRendering().find(el => el === day)
			  if (findetDate) {
				return 'working-day-color'
			  }
		    }
		}
	  },
    },
}
</script>

<style scoped>
.selected {
    background-color: #666060
}

.working-day-color {
    background-color: #00d90087;
    border: none;
    border-bottom: 1px solid black;
}

.weekendDay {
    background-color: #f3f2f2;
}

.vacationDay {
    background-color: #999386;
}

.sickDay {
    background-color: #da7070;
}
</style>