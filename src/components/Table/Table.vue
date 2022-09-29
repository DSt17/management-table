<template>
    <div class="table-box">
	  
	  
	  <div class="table-header">
		<table>
		    <TableHeader
			    :dayInAMonths="dayInAMonths"
			    :options="options"
			    :selected="selected"
			    @AddNewUser="AddNewUser"
			    @sortedByName="sortedByName"
			    @optionSelect="optionSelect"
			    @filteredValue="changeFilteredValue"
		    />
		</table>
	  </div>
	  <div class="scroll-table-body">
		<table>
		    <TableDateRow
			    v-for="(item,idx) in filteredState()"
			    :key="idx"
			    :item="item"
			    :dayInAMonths="dayInAMonths"
		    />
		</table>
	  </div>
    </div>
</template>


<script>
import TableDateRow from '@/components/Table/TableDateRow'
import TableHeader from '@/components/Table/TableHeader'
import vSelect from '@/components/Table/vSelect'

export default {
    data() {
	  return {
		options: [
		    {name: 'All', value: 1},
		    {name: 'in Process', value: 2},
		    {name: 'Free', value: 3},
		],
		selected: 'All',
		filteredValue: '',
		sortClicked: true,
		dayInAMonths: 31,
		state: [
		    {
			  id: 1,
			  title: "Katrin Flanders",
			  status: 'in Process',
			  projects: [
				{task: "Lending", customer: "Google", workingTimeHours: 10, dayStart: 12, dayFinish: 15},
				{task: "Update", customer: "IBM", workingTimeHours: 15, dayStart: 20, dayFinish: 29},
				{task: "Reset", customer: "Microsoft", workingTimeHours: 2, dayStart: 1, dayFinish: 3}
			  ]
		    },
		    {
			  id: 2,
			  title: "Jon Smith",
			  status: 'Free',
			  projects: [
				{task: "SPA", customer: "Google", workingTimeHours: 4, dayStart: 3, dayFinish: 11},
				{task: "Settings", customer: "FuryLion", workingTimeHours: 8, dayStart: 2, dayFinish: 12}
			  ]
		    },
		    {
			  id: 3,
			  title: "Jet Li ",
			  status: 'Free',
			  projects: [
				{task: "SPA", customer: "IBM", workingTimeHours: 5, dayStart: 5, dayFinish: 9},
				{task: "Settings", customer: "Yandex", workingTimeHours: 0, dayStart: 0, dayFinish: 0}]
		    },
		    {
			  id: 4,
			  title: "Katya Bloom",
			  status: 'Free',
			  projects: [
				{task: "SPA", customer: "Google", workingTimeHours: 0, dayStart: 0, dayFinish: 0},
				{task: "SPA", customer: "IBM", workingTimeHours: 0, dayStart: 0, dayFinish: 0},
				{task: "Settings", customer: "Microsoft", workingTimeHours: 10, dayStart: 12, dayFinish: 15}]
		    },
		    {
			  id: 5,
			  title: "Chack Jones",
			  status: 'in Process',
			  projects: [
				{task: "SPA", customer: "Google", workingTimeHours: 2, dayStart: 2, dayFinish: 6,},
				{task: "Settings", customer: "FuryLion", workingTimeHours: 0, dayStart: 0, dayFinish: 0}
			  ]
		    },
		    {
			  id: 6, title: "Boris Grechka ", projects: [
				{task: "SPA", customer: "IBM"}, {task: "Settings", customer: "Yandex"}]
		    },
		    {
			  id: 7,
			  title: "Pavel Poddubniy",
			  status: 'in Process',
			  projects: [
				{task: "SPA", customer: "Google"}, {
				    task: "SPA",
				    customer: "IBM",
				    workingTimeHours: 5,
				    dayStart: 5,
				    dayFinish: 9
				},
				{task: "Settings", customer: "Microsoft", workingTimeHours: 10, dayStart: 12, dayFinish: 15}]
		    },
		    {
			  id: 8,
			  title: "Poul Blond",
			  status: 'in Process',
			  projects: [
				{task: "SPA", customer: "Google", dayStart: 2, dayFinish: 6, workingTimeHours: 10},
				{task: "Settings", customer: "FuryLion", dayStart: 7, dayFinish: 19, workingTimeHours: 30}]
		    },
		    {
			  id: 9,
			  title: "Jeck Loony ",
			  projects: [
				{task: "SPA", customer: "IBM", workingTimeHours: 0, dayStart: 0, dayFinish: 0},
				{task: "Settings", customer: "Yandex", workingTimeHours: 5, dayStart: 5, dayFinish: 9,}]
		    },
		    {
			  id: 10,
			  title: "Elizavetta Cruz",
			  projects: [
				{task: "SPA", customer: "Google", workingTimeHours: 0, dayStart: 0, dayFinish: 0,},
				{task: "SPA", customer: "IBM", workingTimeHours: 24, dayStart: 2, dayFinish: 17,},
				{task: "Settings", customer: "Microsoft", workingTimeHours: 10, dayStart: 12, dayFinish: 15,}]
		    },
		]
	  }
    },
    methods: {
	  sortedByName() {
		if (this.sortClicked) {
		    this.state.sort((a, b) => a.title > b.title ? 1 : -1)
		    this.sortClicked = !this.sortClicked
		} else {
		    this.state.sort((a, b) => a.title > b.title ? -1 : 1)
		    this.sortClicked = !this.sortClicked
		}
	  },
	  filteredState() {
		if (this.selected === 'All') {
		    return this.state
		}
		if (this.selected === 'in Process') {
		    return this.state.filter(el => el.status === 'in Process')
		}
		if (this.selected === 'Free') {
		    return this.state.filter(el => el.status === 'Free')
		}
	  },
	  optionSelect(option) {
		console.log(option)
		this.selected = option.name
	  },
	  AddNewUser(newUser) {
		this.state.unshift(newUser)
	  },
	  changeFilteredValue(value) {
		return this.filteredState().filter(el => el.title.toUpperCase().indexOf(value.toUpperCase()) > -1)
	  }
    },
    
    components: {TableDateRow, TableHeader, vSelect}
}

</script>


<style>

.table-header table {
    table-layout: fixed;
    border-collapse: collapse
}

.scroll-table-body {
    height: 350px;
    overflow-x: auto;
}

.scroll-table-body table {
    border-collapse: collapse;
    border-bottom: 1px solid black
}

::-webkit-scrollbar {
    width: 0;
}

::-webkit-scrollbar-track {
    box-shadow: inset 0 0 6px rgba(0, 0, 0, 0.3);
}

::-webkit-scrollbar-thumb {
    box-shadow: inset 0 0 6px rgba(0, 0, 0, 0.3);
}

.table-box {
    width: 1030px;
    box-shadow: 0 8px 12px 0 rgba(0, 0, 0, 0.2);
}

</style>
