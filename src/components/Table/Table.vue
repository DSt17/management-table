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
			    @rangeDayArray="rangeDayArray"
			    @clickedCalendar="setCLickedCalendar"
		    />
		</table>
	  </div>
	  <div class="scroll-table-body">
		<table>
		    <TableDateRow
			    v-for="(item,idx) in changeFilteredValue()"
			    :key="idx"
			    :item="item"
			    :dayInAMonths="dayInAMonths"
			    :rangeDayArray="rangeDay"
			    :clickedCalendarValue="cLickedCalendar"
		    
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
		dayInAMonths: [
		    "10/1/2022", "10/2/2022", "10/3/2022", "10/4/2022", "10/5/2022", "10/6/2022", "10/7/2022", "10/8/2022", "10/9/2022", "10/10/2022", "10/11/2022", "10/12/2022", "10/13/2022", "10/14/2022", "10/15/2022", "10/16/2022", "10/17/2022", "10/18/2022", "10/19/2022", "10/20/2022", "10/21/2022", "10/22/2022", "10/23/2022", "10/24/2022", "10/25/2022", "10/26/2022", "10/27/2022", "10/28/2022", "10/29/2022", "10/30/2022", "10/31/2022"
		],
		rangeDay: [],
		cLickedCalendar: false,
		state: [
		    {
			  id: 1,
			  title: "Katrin Flanders",
			  status: 'in Process',
			  projects: [
				{
				    task: "Lending",
				    customer: "Google",
				    workingTimeHours: 10,
				    dayStart: "10/12/2022",
				    dayFinish: "10/17/2022"
				},
				{
				    task: "Update",
				    customer: "IBM",
				    workingTimeHours: 15,
				    dayStart: "10/20/2022",
				    dayFinish: "10/31/2022"
				},
				{
				    task: "Reset",
				    customer: "Microsoft",
				    workingTimeHours: 2,
				    dayStart: "10/3/2022",
				    dayFinish: "10/7/2022"
				}
			  ]
		    },
		    {
			  id: 2,
			  title: "Jon Smith",
			  status: 'Free',
			  projects: [
				{
				    task: "SPA",
				    customer: "Google",
				    workingTimeHours: 4,
				    dayStart: "10/3/2022",
				    dayFinish: "10/11/2022"
				},
				{
				    task: "Settings",
				    customer: "FuryLion",
				    workingTimeHours: 8,
				    dayStart: "10/4/2022",
				    dayFinish: "10/12/2022"
				}
			  ]
		    },
		    {
			  id: 3,
			  title: "Jet Li ",
			  status: 'Free',
			  projects: [
				{
				    task: "SPA",
				    customer: "IBM",
				    workingTimeHours: 5,
				    dayStart: "10/5/2022",
				    dayFinish: "10/11/2022"
				},
				{task: "Settings", customer: "Yandex", workingTimeHours: 0, dayStart: '', dayFinish: ''}]
		    },
		    {
			  id: 4,
			  title: "Katya Bloom",
			  status: 'Free',
			  projects: [
				{task: "SPA", customer: "Google", workingTimeHours: 0, dayStart: '', dayFinish: ''},
				{task: "SPA", customer: "IBM", workingTimeHours: 0, dayStart: '', dayFinish: ''},
				{
				    task: "Settings",
				    customer: "Microsoft",
				    workingTimeHours: 10,
				    dayStart: "10/12/2022",
				    dayFinish: "10/15/2022"
				}]
		    },
		    {
			  id: 5,
			  title: "Chack Jones",
			  status: 'in Process',
			  projects: [
				{
				    task: "SPA",
				    customer: "Google",
				    workingTimeHours: 2,
				    dayStart: "10/4/2022",
				    dayFinish: "10/6/2022",
				},
				{task: "Settings", customer: "FuryLion", workingTimeHours: 0, dayStart: '', dayFinish: ''}
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
				{task: "SPA", customer: "Google"},
				{
				    task: "SPA",
				    customer: "IBM",
				    workingTimeHours: 5,
				    dayStart: "10/5/2022",
				    dayFinish: "10/14/2022"
				},
				{
				    task: "Settings",
				    customer: "Microsoft",
				    workingTimeHours: 10,
				    dayStart: "10/12/2022",
				    dayFinish: "10/17/2022"
				}]
		    },
		    {
			  id: 8,
			  title: "Poul Blond",
			  status: 'in Process',
			  projects: [
				{
				    task: "SPA",
				    customer: "Google",
				    dayStart: "10/3/2022",
				    dayFinish: "10/6/2022",
				    workingTimeHours: 10
				},
				{
				    task: "Settings",
				    customer: "FuryLion",
				    dayStart: "10/7/2022",
				    dayFinish: "10/19/2022",
				    workingTimeHours: 30
				}]
		    },
		    {
			  id: 9,
			  title: "Jeck Loony ",
			  projects: [
				{task: "SPA", customer: "IBM", workingTimeHours: 0, dayStart: '', dayFinish: ''},
				{
				    task: "Settings",
				    customer: "Yandex",
				    workingTimeHours: 5,
				    dayStart: "10/5/2022",
				    dayFinish: "10/12/2022",
				}]
		    },
		    {
			  id: 10,
			  title: "Elizavetta Cruz",
			  projects: [
				{task: "SPA", customer: "Google", workingTimeHours: 0, dayStart: '', dayFinish: '',},
				{
				    task: "SPA",
				    customer: "IBM",
				    workingTimeHours: 24,
				    dayStart: "10/4/2022",
				    dayFinish: "10/17/2022",
				},
				{
				    task: "Settings",
				    customer: "Microsoft",
				    workingTimeHours: 10,
				    dayStart: "10/12/2022",
				    dayFinish: "10/14/2022",
				}]
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
	  rangeDayArray(array) {
		this.rangeDay = array
	  },
	  setCLickedCalendar(value) {
		this.cLickedCalendar = value
	  },
	  
	  
	  //------need to fix with Dmitry-------------//
	  changeFilteredValue(value) {
		if (value === undefined) {
		    return this.filteredState()
		}
		if (value.trim() === '') {
		    return this.filteredState()
		} else {
		    const newState = this.filteredState().filter(el => el.title.toUpperCase().indexOf(value.toUpperCase()) > -1)
		    console.log(newState)
		    return newState
		}
	  }
    },
    
    components: {TableDateRow, TableHeader, vSelect}
}

</script>


<style>
.table-header table {
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
