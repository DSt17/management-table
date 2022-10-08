<template>
    <tr>
	  <td style=" font-size: 10px; position: sticky; left: 0; background-color: white; width: 150px;">
		{{ item.task }} ({{ item.customer }})
	  </td>
	  <td v-for="(day,idx) in arrayForRendering()"
		:key="idx"
		:class="workAndWeekendClass(day)">
		<span style="font-size: 10px">
		    {{
			  idx + 1 === new Date(item.dayStart).getDate() ? item.workingTimeHours : ''
		    }}
		</span>
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
	  workAndWeekendClass(day) {
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
			  return new Date(day).getMonth() === new Date(this.item.dayStart).getMonth() ||
			  new Date(day).getMonth() === new Date(this.item.dayFinish).getMonth() ? 'working-day-color' : ''
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