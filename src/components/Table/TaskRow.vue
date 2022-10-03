<template>
    <tr>
	  <td style="width: 150px; font-size: 10px;">
		{{ item.task }} ({{ item.customer }})
	  </td>
	  <td v-for="(day,idx) in arrayForRendering()"
		:key="idx"
		:class="workAndweekendClass(day)">
		<span style="font-size: 12px">{{
			  idx + 1 === new Date(item.dayStart).getDate() ? item.workingTimeHours : ''
		    }}</span>
	  </td>
    </tr>
</template>

<script>
export default {
    name: "TaskRow",
    props: ["item", "dayInAMonths", "rangeDayArray", "clickedCalendar"],
    methods: {
	  arrayForRendering() {
		if (this.clickedCalendar === true) {
		    return this.rangeDayArray
		} else {
		    return this.dayInAMonths
		}
	  },
	  workAndweekendClass(day) {
		let weekendDayCount = 0
		if (new Date(day).getDay() === 6 || new Date(day).getDay() === 0) {
		    return 'weekendDay'
		}
		if (new Date(day).getDate() >= new Date(this.item.dayStart).getDate()
			&&
			new Date(day).getDate() <= new Date(this.item.dayFinish).getDate() + weekendDayCount) {
		    if (new Date(day).getDay() === 6) {
			  weekendDayCount += 1
			  return 'weekendDay'
		    }
		    if (new Date(day).getDay() === 0) {
			  weekendDayCount += 1
			  return 'weekendDay'
		    } else {
			  debugger
			  return 'working-day-color'
		    }
		}
	  }
	  
    }
}
</script>

<style scoped>
.working-day-color {
    background-color: #00d90087;
    border: none;
    border-bottom: 1px solid black;
}

.weekendDay {
    background-color: #f3f2f2;
}
</style>