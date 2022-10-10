<template>
    
    
    <div class="table-box">
	  <div class="table-header">
		<TableHeader
			:item="item"
			:taskPopupVisible="addTaskPopupVisible"
			:options="options"
			:selected="selected"
			@AddNewUser="AddNewUser"
			@AddNewTask="AddNewTask"
			@AddNewTaskInCompany="AddNewTaskInCompany"
			@sortedByName="sortedByName"
			@optionSelect="optionSelect"
			@filteredValue="changeFilteredValue"
			@rangeDayArray="rangeDayArray"
			@clickedCalendarValue="setCLickedCalendar"
			@closeAddTaskPopup="closeAddTaskPopup"
			@toggleChecked="toggleChecked"
		/>
	  
	  </div>
	  <div class="scrollTableBody">
		<table id="table">
		    <tr style="position: sticky; top: 0; background: white; z-index: 2">
			  <td class="monthRowHeader" :colspan="colspan() + 1">
				<div class="monthRowValue">
				<span>
				    {{
					  rangeDay.length > 0 ? months[new Date(rangeDay[1]).getMonth()] :
						  months[new Date(dayInAMonths[1]).getMonth()]
				    }}
				</span>
				</div>
			  </td>
		    </tr>
		    <tr style="position: sticky; top: 18px; background: white; z-index: 2 ">
			  <td style="position: sticky; left: 0; background-color: white; ">
				<div style="display: flex; flex-direction: row; justify-content: space-around">
				    <div style="border-right: 1px solid black;width: 50%">Name</div>
				    <div style="width:50%">Client</div>
				</div>
			  </td>
			  <td
				  v-for="(day,idx) in arrayForRendering()"
				  :key="idx"
				  :class="new Date(day).getDay() === 6 || new Date(day).getDay() === 0 ? 'weekendDay' : '' "
			  ><span style="font-size: 12px; font-weight: bold;">{{ new Date(day).getDate() }}</span>
			  </td>
		    </tr>
		    
		    <TableDateRow
			    v-for="(item,idx) in filteredList"
			    :key="idx"
			    :item="item"
			    :dayInAMonths="dayInAMonths"
			    :rangeDayArray="rangeDay"
			    :clickedCalendarValue="cLickedCalendar"
			    @showAddTaskPopup="showAddTaskPopup"
		    />
		</table>
	  </div>
	  <div class="scrollFooterBox">
		<input type="range" class="slider"
			 step="1"
			 min="1"
			 max="100"
			 v-model.number="maxTableSize"
			 @change="changeTableSize"
		>
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
		months: ['January', 'February', 'March', 'April', 'May', 'June', 'July', 'August', 'September', 'October', 'November', 'December'],
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
		],
		maxTableSize: 50,
		addTaskPopupVisible: false,
		item: {},
		customers: ['FuryLion', 'Google', 'IBM', 'Microsoft', 'Yandex'],
		stateCompany: [],
		toggleCheckedValue: false,
		filterInputValue: ''
	  }
    },
    methods: {
	  sortedByName() {
		if (this.sortClicked) {
		    this.filteredState().sort((a, b) => a.title > b.title ? 1 : -1)
		    this.sortClicked = !this.sortClicked
		} else {
		    this.filteredState().sort((a, b) => a.title > b.title ? -1 : 1)
		    this.sortClicked = !this.sortClicked
		}
	  },
	  filteredState() {
		if (this.toggleCheckedValue === false) {
		    if (this.selected === 'All') {
			  return this.state
		    }
		    if (this.selected === 'in Process') {
			  return this.state.filter(el => el.status === 'in Process')
		    }
		    if (this.selected === 'Free') {
			  return this.state.filter(el => el.status === 'Free')
		    }
		} else {
		    if (this.selected === 'All') {
			  return this.stateCompany
		    }
		    if (this.selected === 'in Process') {
			  return this.stateCompany.filter(el => el.status === 'in Process')
		    }
		    if (this.selected === 'Free') {
			  return this.stateCompany.filter(el => el.status === 'Free')
		    }
		}
	  },
	  optionSelect(option) {
		console.log(option)
		this.selected = option.name
	  },
	  AddNewUser(newUser) {
		this.state.unshift(newUser)
	  },
	  AddNewTask(newTask, userId) {
		let findedUser = this.state.find(el => el.id === userId)
		findedUser.projects.push(newTask)
	  },
	  AddNewTaskInCompany(newTask, CompanyTitle) {
		let findedCompany = this.stateCompany.find(el => el.title === CompanyTitle)
		findedCompany.projects.push(newTask)
	  },
	  rangeDayArray(rangeDayArray) {
		this.rangeDay = rangeDayArray
	  },
	  setCLickedCalendar(value) {
		this.cLickedCalendar = value
	  },
	  arrayForRendering() {
		if (this.cLickedCalendar === true) {
		    return this.rangeDay
		    
		} else {
		    return this.dayInAMonths
		}
	  },
	  changeTableSize() {
		let table = document.getElementById('table')
		table.width = 51500 / this.maxTableSize
		if (table.width < 1030) {
		    table.width = 1030
		}
		
		
	  },
	  showAddTaskPopup(item) {
		this.addTaskPopupVisible = true
		this.item = item
	  },
	  closeAddTaskPopup() {
		this.addTaskPopupVisible = false
	  },
	  colspan() {
		if (this.rangeDay.length > 0) {
		    return this.rangeDay.length
		} else {
		    return this.dayInAMonths.length
		}
	  },
	  toggleChecked(value) {
		this.toggleCheckedValue = value
		if (value === true) {
		    this.stateCompany = []
		    for (let i = 0; i < this.customers.length; i++) {
			  let newObj = {
				id: new Date().getTime(),
				title: this.customers[i],
				status: 'in Process',
				projects: []
			  }
			  let customerProjects = this.state.map((el) => {
				return {...el, projects: el.projects.filter(el => el.customer === newObj.title)}
			  })
			  let tasksArray = customerProjects.map(user => user.projects.map(task => {
				return {...task, customer: user.title}
			  }))
			  for (let j = 0; j < tasksArray.length; j++) {
				if (tasksArray[j].length > 0) {
				    newObj.projects.push(...tasksArray[j])
				}
			  }
			  if (newObj.projects.length > 0) {
				this.stateCompany.push(newObj)
			  }
		    }
		}
	  },
	  changeFilteredValue(value) {
		this.filterInputValue = value
	  }
    },
    computed: {
	  filteredList: function () {
		let InputValue = this.filterInputValue.toUpperCase()
		return this.filteredState().filter(function (elem) {
		    if (InputValue === '') return true;
		    else return elem.title.indexOf(InputValue) > -1;
		})
	  }
    },
    
    components: {
	  TableDateRow, TableHeader, vSelect
    }
}

</script>


<style>
.monthRowHeader {
    height: 15px;
    background-color: #346977
}

.monthRowValue {
    width: 100%;
    height: 15px;
    display: flex;
    flex-direction: row;
    justify-content: center;
    color: white
}

.scrollTableBody {
    height: 350px;
    overflow: scroll;
}

.scrollTableBody table {
    border-collapse: separate;
    border-spacing: 0;
    border-bottom: 1px solid black;
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

.weekendDay {
    background-color: #f3f2f2;
}

td {
    width: 26px;
    min-width: 10px;
    height: 25px;
    min-height: 10px;
    
}

.scrollFooterBox {
    width: 100%;
    height: 22px;
    background-color: #346977;
}

.slider {
    -webkit-appearance: none;
    height: 3px;
    background: #fdfcfc;
    outline: none;
    opacity: 0.3;
    -webkit-transition: .2s;
    transition: opacity .2s;
    width: 300px
}

.slider:hover {
    opacity: 1;
}

.slider::-webkit-slider-thumb {
    appearance: none;
    width: 11px;
    height: 11px;
    border-radius: 100%;
    border: 1px solid black;
    background: white;
    cursor: pointer;
}
</style>
