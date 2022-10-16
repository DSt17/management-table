<template>
    <div class="table-box">
	  <div class="table-header">
		<TableHeader
			:item="item"
			:customer="customer"
			:ArraySelectedDays="ArraySelectedDays"
			:taskPopupVisible="addTaskPopupVisible"
			:taskCalendarPopupVisible="addTaskCalendarPopupVisible"
			:addTaskCalendarPopupVisibleOnClickUnderTheHading="addTaskCalendarPopupVisibleOnClickUnderTheHading"
			:options="options"
			:selected="selected"
			@AddNewUser="AddNewUser"
			@AddNewTask="AddNewTask"
			@addTaskOnClickUnderTheHading="addTaskOnClickUnderTheHading"
			@AddNewTaskClickedCalendar="AddNewTaskClickedCalendar"
			@AddNewTaskInCompany="AddNewTaskInCompany"
			@sortedByName="sortedByName"
			@optionSelect="optionSelect"
			@filteredValue="changeFilteredValue"
			@rangeDayArray="rangeDayArray"
			@clickedCalendarValue="setCLickedCalendar"
			@closeAddTaskPopup="closeAddTaskPopup"
			@closeAddTaskCalendarPopup="closeAddTaskCalendarPopup"
			@toggleChecked="toggleChecked"
			@AddNewUserInACompany="AddNewUserInACompany"
			@currentMonth="currentMonth"
		/>
	  </div>
	  <div class="scrollTableBody">
		<table id="table">
		    <tr style="position: sticky; top: 0; background: white; z-index: 2">
			  <td class="monthRowHeader" :colspan="colspan()">
				<div class="monthRowValue" id="monthRow">
				<span>
				    {{
					  this.rangeDay.length > 0 ? this.monthsName[new Date(this.rangeDay[0]).getMonth()] : this.month
				    }}
				</span>
				</div>
			  </td>
		    </tr>
		    <tr style="position: sticky; top: 18px; background: white; z-index: 2 ">
			  <td style="position: sticky; left: 0; background-color: white; ">
				<div style="display: flex; flex-direction: row; justify-content: space-around">
				    <div style="border-right: 1px solid black;width: 50%">
					  <span v-if="toggleCheckedValue">Client</span>
					  <span v-if="!toggleCheckedValue">People</span></div>
				    <div style="width:50%">
					  <span v-if="toggleCheckedValue">People</span>
					  <span v-if="!toggleCheckedValue">Client</span>
				    
				    </div>
				</div>
			  </td>
			  <td
				  v-for="(day,idx) in arrayForRendering()"
				  :key="idx"
				  :class="new Date(day).getDay() === 6 || new Date(day).getDay() === 0 ? 'weekendDay' : '' "
			  >
				<div style="min-width: 15px">
				<span style="font-size: 12px; font-weight: bold;">
				{{ new Date(day).getDate() }}
			  </span>
				</div>
			  </td>
		    </tr>
		    
		    <TableDateRow
			    v-for="(item,idx) in filteredList"
			    :month="month"
			    :key="idx"
			    :item="item"
			    :monthsState="monthsState"
			    :rangeDayArray="rangeDay"
			    :clickedCalendarValue="cLickedCalendar"
			    :toggleCheckedValue="toggleCheckedValue"
			    @showAddTaskPopup="showAddTaskPopup"
			    @showAddTaskCalendarPopupVisible="showAddTaskCalendarPopupVisible"
			    @showAddTaskOnClickUnderTheHading="showAddTaskOnClickUnderTheHading"
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
		monthsState: {
		    "January": ["1/1/2022", "1/2/2022", "1/3/2022", "1/4/2022", "1/5/2022", "1/6/2022",
			  "1/7/2022", "1/8/2022", "1/9/2022", "1/10/2022", "1/11/2022", "1/12/2022",
			  "1/13/2022", "1/14/2022", "1/15/2022", "1/16/2022", "1/17/2022", "1/18/2022",
			  "1/19/2022", "1/20/2022", "1/21/2022", "1/22/2022", "1/23/2022", "1/24/2022",
			  "1/25/2022", "1/26/2022", "1/27/2022", "1/28/2022", "1/29/2022", "1/30/2022", "1/31/2022"
		    ],
		    "February": ["2/1/2022", "2/2/2022", "2/3/2022", "2/4/2022", "2/5/2022", "2/6/2022",
			  "2/7/2022", "2/8/2022", "2/9/2022", "2/10/2022", "2/11/2022", "2/12/2022",
			  "2/13/2022", "2/14/2022", "2/15/2022", "2/16/2022", "2/17/2022", "2/18/2022",
			  "2/19/2022", "2/20/2022", "2/21/2022", "2/22/2022", "2/23/2022", "2/24/2022",
			  "2/25/2022", "2/26/2022", "2/27/2022", "2/28/2022"
		    ],
		    "March": ["3/1/2022", "3/2/2022", "4/3/2022", "3/4/2022", "3/5/2022", "3/6/2022",
			  "3/7/2022", "3/8/2022", "3/9/2022", "3/10/2022", "3/11/2022", "3/12/2022",
			  "3/13/2022", "3/14/2022", "3/15/2022", "3/16/2022", "3/17/2022", "3/18/2022",
			  "3/19/2022", "3/20/2022", "3/21/2022", "3/22/2022", "3/23/2022", "3/24/2022",
			  "3/25/2022", "3/26/2022", "3/27/2022", "3/28/2022", "3/29/2022", "3/30/2022", "3/31/2022"
		    ],
		    "April": ["4/1/2022", "4/2/2022", "4/3/2022", "4/4/2022", "4/5/2022", "4/6/2022",
			  "4/7/2022", "4/8/2022", "4/9/2022", "4/10/2022", "4/11/2022", "4/12/2022",
			  "4/13/2022", "4/14/2022", "4/15/2022", "4/16/2022", "4/17/2022", "4/18/2022",
			  "4/19/2022", "4/20/2022", "4/21/2022", "4/22/2022", "4/23/2022", "4/24/2022",
			  "4/25/2022", "4/26/2022", "4/27/2022", "4/28/2022", "4/29/2022", "4/30/2022"
		    ],
		    "May": ["5/1/2022", "5/2/2022", "5/3/2022", "5/4/2022", "5/5/2022", "5/6/2022",
			  "5/7/2022", "5/8/2022", "5/9/2022", "5/10/2022", "5/11/2022", "5/12/2022",
			  "5/13/2022", "5/14/2022", "5/15/2022", "5/16/2022", "5/17/2022", "5/18/2022",
			  "5/19/2022", "5/20/2022", "5/21/2022", "5/22/2022", "5/23/2022", "5/24/2022",
			  "5/25/2022", "5/26/2022", "5/27/2022", "5/28/2022", "5/29/2022", "5/30/2022", "5/31/2022"
		    ],
		    "June": ["6/1/2022", "6/2/2022", "6/3/2022", "6/4/2022", "6/5/2022", "6/6/2022",
			  "6/7/2022", "6/8/2022", "6/9/2022", "6/10/2022", "6/11/2022", "6/12/2022",
			  "6/13/2022", "6/14/2022", "6/15/2022", "6/16/2022", "6/17/2022", "6/18/2022",
			  "6/19/2022", "6/20/2022", "6/21/2022", "6/22/2022", "6/23/2022", "6/24/2022",
			  "6/25/2022", "6/26/2022", "6/27/2022", "6/28/2022", "6/29/2022", "6/30/2022"
		    ],
		    "July": ["7/1/2022", "7/2/2022", "7/3/2022", "7/4/2022", "7/5/2022", "7/6/2022",
			  "7/7/2022", "7/8/2022", "7/9/2022", "7/10/2022", "7/11/2022", "7/12/2022",
			  "7/13/2022", "7/14/2022", "7/15/2022", "7/16/2022", "7/17/2022", "7/18/2022",
			  "7/19/2022", "7/20/2022", "7/21/2022", "7/22/2022", "7/23/2022", "7/24/2022",
			  "7/25/2022", "7/26/2022", "7/27/2022", "7/28/2022", "7/29/2022", "7/30/2022", "7/31/2022"
		    ],
		    "August": ["8/1/2022", "8/2/2022", "8/3/2022", "8/4/2022", "8/5/2022", "8/6/2022",
			  "8/7/2022", "8/8/2022", "8/9/2022", "8/10/2022", "8/11/2022", "8/12/2022",
			  "8/13/2022", "8/14/2022", "8/15/2022", "8/16/2022", "8/17/2022", "8/18/2022",
			  "8/19/2022", "8/20/2022", "8/21/2022", "8/22/2022", "8/23/2022", "8/24/2022",
			  "8/25/2022", "8/26/2022", "8/27/2022", "8/28/2022", "8/29/2022", "8/30/2022", "8/31/2022"
		    ],
		    "September": ["9/1/2022", "9/2/2022", "9/3/2022", "9/4/2022", "9/5/2022", "9/6/2022",
			  "9/7/2022", "9/8/2022", "9/9/2022", "9/10/2022", "9/11/2022", "9/12/2022",
			  "9/13/2022", "9/14/2022", "9/15/2022", "9/16/2022", "9/17/2022", "9/18/2022",
			  "9/19/2022", "9/20/2022", "9/21/2022", "9/22/2022", "9/23/2022", "9/24/2022",
			  "9/25/2022", "9/26/2022", "9/27/2022", "9/28/2022", "9/29/2022", "9/30/2022"
		    ],
		    "October": ["10/1/2022", "10/2/2022", "10/3/2022", "10/4/2022", "10/5/2022", "10/6/2022",
			  "10/7/2022", "10/8/2022", "10/9/2022", "10/10/2022", "10/11/2022", "10/12/2022",
			  "10/13/2022", "10/14/2022", "10/15/2022", "10/16/2022", "10/17/2022", "10/18/2022",
			  "10/19/2022", "10/20/2022", "10/21/2022", "10/22/2022", "10/23/2022", "10/24/2022",
			  "10/25/2022", "10/26/2022", "10/27/2022", "10/28/2022", "10/29/2022", "10/30/2022", "10/31/2022"
		    ],
		    "November": ["11/1/2022", "11/2/2022", "11/3/2022", "11/4/2022", "11/5/2022", "11/6/2022",
			  "11/7/2022", "11/8/2022", "11/9/2022", "11/10/2022", "11/11/2022", "11/12/2022",
			  "11/13/2022", "11/14/2022", "11/15/2022", "11/16/2022", "11/17/2022", "11/18/2022",
			  "11/19/2022", "11/20/2022", "11/21/2022", "11/22/2022", "11/23/2022", "11/24/2022",
			  "11/25/2022", "11/26/2022", "11/27/2022", "11/28/2022", "11/29/2022", "11/30/2022"
		    ],
		    "December": ["12/1/2022", "12/2/2022", "12/3/2022", "12/4/2022", "12/5/2022", "12/6/2022",
			  "12/7/2022", "12/8/2022", "12/9/2022", "12/10/2022", "12/11/2022", "12/12/2022",
			  "12/13/2022", "12/14/2022", "12/15/2022", "12/16/2022", "12/17/2022", "12/18/2022",
			  "12/19/2022", "12/20/2022", "12/21/2022", "12/22/2022", "12/23/2022", "12/24/2022",
			  "12/25/2022", "12/26/2022", "12/27/2022", "12/28/2022", "12/29/2022", "12/30/2022", "12/31/2022"
		    ]
		    
		},
		monthsName: ['January', 'February', 'March', 'April', 'May', 'June', 'July', 'August', 'September', 'October', 'November', 'December'],
		rangeDay: [],
		cLickedCalendar: false,
		month: 'October',
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
				    dayFinish: "11/22/2022"
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
		addTaskCalendarPopupVisible: false,
		addTaskCalendarPopupVisibleOnClickUnderTheHading: false,
		item: {},
		customers: ['FuryLion', 'Google', 'IBM', 'Microsoft', 'Yandex'],
		stateCompany: [],
		toggleCheckedValue: false,
		filterInputValue: '',
		ArraySelectedDays: [],
		customer: '',
		tableWidth: ''
	  }
    },
    methods: {
	  colspan() {
		if (this.rangeDay.length === 0) {
		    return this.monthsState[this.month].length + 1
		} else {
		    return this.rangeDay.length + 1
		}
	  },
	  currentMonth(month) {
		this.rangeDay = []
		this.month = month
	  },
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
		this.selected = option.name
	  },
	  AddNewUser(newUser) {
		let findedUser = this.state.find(el => el.title === newUser.title)
		if (findedUser) {
		    findedUser.projects.push(...newUser.projects)
		} else this.state.unshift(newUser)
	  },
	  AddNewUserInACompany(newUserInAState, newUserFromCompany, selectedCustomer) {
		let findedCompany = this.stateCompany.find(el => el.title === selectedCustomer)
		findedCompany.projects.push(newUserFromCompany)
		let findedUser = this.state.find(el => el.title === newUserInAState.title)
		if (findedUser) {
		    findedUser.projects.push(...newUserInAState.projects)
		} else {
		    this.state.push(newUserInAState)
		}
	  },
	  AddNewTask(newTask, userId) {
		let findedUser = this.state.find(el => el.id === userId)
		findedUser.projects.push(newTask)
	  },
	  addTaskOnClickUnderTheHading(newTask, userId) {
		let findedUser = this.state.find(el => el.id === userId)
		findedUser.projects.push(newTask)
	  },
	  AddNewTaskClickedCalendar(newTask, userId) {
		let findedUser = this.state.find(el => el.id === userId)
		findedUser.projects.push(newTask)
	  },
	  AddNewTaskInCompany(newUser, newTask) {
		let findedCompany = this.stateCompany.find(el => el.title === newUser.projects[0].customer)
		findedCompany.projects.push(newTask)
		let findedUser = this.state.find(el => el.title === newUser.title)
		if (findedUser) {
		    findedUser.projects.push(...newUser.projects)
		} else {
		    this.state.push(newUser)
		}
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
		    return this.monthsState[this.month]
		}
	  },
	  changeTableSize() {
		let table = document.getElementById('table')
		this.tableWidth = table.width
		table.width = 51500 / this.maxTableSize
		if (table.width < 1030) {
		    table.width = 1030
		}
	  },
	  showAddTaskPopup(item) {
		this.addTaskPopupVisible = true
		this.item = item
	  },
	  showAddTaskCalendarPopupVisible(item, ArraySelectedDays, customer) {
		this.addTaskCalendarPopupVisible = true
		this.item = item
		this.customer = customer
		this.ArraySelectedDays = ArraySelectedDays
	  },
	  showAddTaskOnClickUnderTheHading(item, customer) {
		this.addTaskCalendarPopupVisibleOnClickUnderTheHading = true
		this.item = item
		this.customer = customer
	  },
	  closeAddTaskPopup() {
		this.addTaskPopupVisible = false
		this.addTaskCalendarPopupVisibleOnClickUnderTheHading = false
	  },
	  closeAddTaskCalendarPopup() {
		this.addTaskCalendarPopupVisible = false
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
	  },
	  
    },
    components: {
	  TableDateRow, TableHeader, vSelect
    },
    mounted() {
	  let table = document.getElementById('table')
	  
	  if (table.width < 1030) {
		table.width = 1030
	  }
	  
    },
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
