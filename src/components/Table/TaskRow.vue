<template>
    <tr>
	  <td style="width: 150px; font-size: 10px;">
		{{ item.task }} ({{ item.customer }})
	  </td>
	  <td v-for="(dayInAMonths,idx) in dayInAMonths"
		v-bind:key="idx"
		v-bind:class="idx+1 >= item.dayStart && idx+1 <= item.dayFinish ? 'working-day-color' : '' "
	  >
		<span style="font-size: 12px">{{idx+1 === item.dayStart  ?  item.workingTimeHours : ''}}</span>
	  </td>
    </tr>
</template>

<script>
export default {
    name: "TaskRow",
    props: ["item","dayInAMonths"]
}
</script>

<style scoped>
.working-day-color {
    background-color: #56ee5687;
    border: none;
    border-bottom: 1px solid black;
}
</style>