<template>
    <div>
	  <addUserFormPopup
		  v-if="!toggleChecked && isInfoPopupVisible"
		  @closePopup="closePopup"
		  @AddNewUser="AddNewUser"
	  />
	  <CalendarPopup
		  v-if="isCalendarPopupVisible"
		  @closeCalendarPopup="closeCalendarPopup"
		  @rangeDayOfTheWeekNumberArray="rangeDay"
		  @deyStart="setDeyStart"
		  @dayFinish="setDayFinish"
		  @clickedCalendarMonths="setClickedCalendarMonths"
	  />
	  <addTaskPopup
		  v-if=" !toggleChecked && taskPopupVisible && !addTaskCalendarPopupVisibleOnClickUnderTheHading"
		  @closeAddTaskPopup="closeAddTaskPopup"
		  @AddNewTask="AddNewTask"
		  :item="item"
	  />
	 
	  
	  <addTaskFromCompanyPopup
		  v-if="toggleChecked && taskPopupVisible"
		  @closeAddTaskPopup="closeAddTaskPopup"
		  @AddNewTaskInCompany="AddNewTaskInCompany"
		  :item="item"
	  />
	  <addUserFormCompanyPopup
		  v-if="toggleChecked && isInfoPopupVisible"
		  @closePopup="closePopup"
		  @AddNewUserInACompany="AddNewUserInACompany"
	  />
	  <addTaskCalendarPopup
		  v-if="!toggleChecked && taskCalendarPopupVisible"
		  @closeAddTaskCalendarPopup="closeAddTaskCalendarPopup"
		  @AddNewTaskClickedCalendar="AddNewTaskClickedCalendar"
		  :item="item"
		  :customer="customer"
		  :ArraySelectedDays="ArraySelectedDays"
	  />
	  <addTaskOnClickUnderTheHading
		  v-if=" !toggleChecked && addTaskCalendarPopupVisibleOnClickUnderTheHading"
		  @closeAddTaskPopup="closeAddTaskPopup"
		  @AddNewTask="AddNewTask"
		  :item="item"
		  :customer="customer"
	  />
	  
	  <div class="table-header-navbar-box">
		<div class="header-navbar-navigation-box">
		    <div>
			  <input
				  type="text"
				  placeholder="Search..."
				  style="width: 180px;"
				  v-model="filteredValue"
			  >
		    </div>
		    <div class="switchBox">
			  <span @click="changeToggleFalse">Employee</span>
			  <div style=" display: flex; align-items: center;">
				<input id="switchBoxToggle" type="checkbox"
					 v-model="toggleChecked"
				>
			  </div>
			  <span @click="changeToggleTrue">Company</span>
		    </div>
		    <div class="sort-button"
			   @click="sortedByName">
			  <span>sort </span>
			  <i class="material-icons">sort_by_alpha</i>
		    </div>
		    <div class="filter-button">
			  <vSelect
				  :options="options"
				  @select="optionSelect"
				  :selected="selected"
			  />
			  <i class="material-icons">filter_alt</i>
		    </div>
		    <div class="add-button"
			   @click="showPopupInfo"
		    >
			  <span>Add new task </span>
			  <i class="material-icons">person_add</i>
		    </div>
		    
		    <div class="month-button-box" style="color: white">
			  <span @click="prevMonth"><<</span>
			  <div class="month-button-value">
				<div v-if="clickedCalendar === false">{{ currentMonth }}</div>
				<div v-if="clickedCalendar === true" style="font-size: 10px">
				    {{ dayStart.replace(/-/g, '/') }} - {{ dayFinish.replace(/-/g, '/') }}
				</div>
				<div>
				    <i @click="showCalendarPopupInfo"
					 style="font-size: 20px; padding-top: 4px; cursor: pointer;"
					 class="material-icons">calendar_month</i>
				</div>
			  </div>
			  <span @click="nextMonth">>></span>
		    </div>
		</div>
	  </div>
    </div>

</template>

<script>
import vSelect from '@/components/Table/vSelect'
import addUserFormPopup from '@/components/Table/addUserFormPopup'
import CalendarPopup from '@/components/Table/CalendarPopup'
import addTaskPopup from '@/components/Table/addTaskPopup'
import addTaskFromCompanyPopup from '@/components/Table/addTaskFromCompanyPopup'
import addUserFormCompanyPopup from '@/components/Table/addUserFormCompanyPopup'
import addTaskCalendarPopup from '@/components/Table/addTaskCalendarPopup'
import addTaskOnClickUnderTheHading from '@/components/Table/addTaskOnClickUnderTheHading'

export default {
    name: "TableHeader",
    props: ["options", "selected", "taskPopupVisible", "item", "taskCalendarPopupVisible", "ArraySelectedDays","customer","addTaskCalendarPopupVisibleOnClickUnderTheHading"],
    data() {
	  return {
		isInfoPopupVisible: false,
		isCalendarPopupVisible: false,
		addTaskCalendarPopupVisible: false,
		clickedCalendar: false,
		toggleChecked: '',
		filteredValue: '',
		rangeDayArray: [],
		dayStart: '',
		dayFinish: '',
		currentMonth: 'October',
		currentYear: 2022,
		months: ["January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"],
	  }
    },
    methods: {
	  sortedByName() {
		this.$emit('sortedByName')
	  },
	  optionSelect(option) {
		this.$emit('optionSelect', option)
	  },
	  showPopupInfo() {
		this.isInfoPopupVisible = true
	  },
	  closePopup() {
		this.isInfoPopupVisible = false
	  },
	  closeAddTaskCalendarPopup() {
		this.addTaskCalendarPopupVisible = false
		this.$emit('closeAddTaskCalendarPopup')
	  },
	  AddNewUser(newUser) {
		this.$emit('AddNewUser', newUser)
	  },
	  AddNewUserInACompany(newUserInAState, newUserFromCompany, selectedCustomer) {
		this.$emit('AddNewUserInACompany', newUserInAState, newUserFromCompany, selectedCustomer)
	  },
	  AddNewTask(newTask, userId) {
		debugger
		this.$emit('AddNewTask', newTask, userId)
	  },
	  AddNewTaskClickedCalendar(newTask, userId) {
		debugger
		this.$emit('AddNewTaskClickedCalendar', newTask, userId)
	  },
	  AddNewTaskInCompany(newUser, newTask) {
		this.$emit('AddNewTaskInCompany', newUser, newTask)
	  },
	  showCalendarPopupInfo() {
		this.isCalendarPopupVisible = true
	  },
	  closeCalendarPopup() {
		this.isCalendarPopupVisible = false
	  },
	  closeAddTaskPopup() {
		this.$emit('closeAddTaskPopup')
	  },
	  rangeDay(rangeDayArray) {
		this.rangeDayArray = rangeDayArray
		this.$emit('rangeDayArray', this.rangeDayArray)
	  },
	  prevMonth() {
		this.clickedCalendar = false
		this.$emit('clickedCalendarValue', this.clickedCalendar)
		if (this.months.indexOf(this.currentMonth) > 0) {
		    this.currentMonth = this.months[this.months.indexOf(this.currentMonth) - 1]
		} else {
		    this.clickedCalendar = false
		    this.currentMonth = "December"
		    this.currentYear--
		}
	  },
	  nextMonth() {
		this.clickedCalendar = false
		this.$emit('clickedCalendarValue', this.clickedCalendar)
		if (this.months.indexOf(this.currentMonth) < 11) {
		    this.currentMonth = this.months[this.months.indexOf(this.currentMonth) + 1]
		} else {
		    this.clickedCalendar = false
		    this.currentMonth = "January"
		    this.currentYear++
		}
	  },
	  setDeyStart(dayStart) {
		this.dayStart = dayStart
	  },
	  setDayFinish(dayFinish) {
		this.dayFinish = dayFinish
	  },
	  setClickedCalendarMonths(value) {
		this.clickedCalendar = value
		this.$emit('clickedCalendarValue', this.clickedCalendar)
	  },
	  changeToggleFalse() {
		this.toggleChecked = false
		this.$emit('toggleChecked', this.toggleChecked)
	  },
	  changeToggleTrue() {
		this.toggleChecked = true
		this.$emit('toggleChecked', this.toggleChecked)
	  }
    },
    watch: {
	  filteredValue(filteredValue) {
		this.$emit('filteredValue', filteredValue)
	  },
	  toggleChecked() {
		this.$emit('toggleChecked', this.toggleChecked)
	  }
    },
    components: {
	  vSelect,
	  addUserFormPopup,
	  CalendarPopup,
	  addTaskPopup,
	  addTaskFromCompanyPopup,
	  addUserFormCompanyPopup,
	  addTaskCalendarPopup,
	  addTaskOnClickUnderTheHading
    }
}
</script>

<style scoped>
td {
    font-weight: bold;
    font-size: 15px;
}

.table-header-navbar-box {
    border: 1px solid black;
    height: 25px;
    background-color: #346977;
    display: flex;
    flex-direction: column;
    justify-content: center
}

.header-navbar-navigation-box {
    display: flex;
    flex-direction: row;
    align-items: center;
    justify-content: start
}

.sort-button {
    color: white;
    margin-left: 20px;
    width: 60px;
    border: 1px solid white;
    border-radius: 3%;
    display: flex;
    justify-content: space-around;
    cursor: pointer;
}

.add-button {
    color: white;
    margin-left: 40px;
    width: 200px;
    border: 1px solid white;
    border-radius: 2%;
    display: flex;
    justify-content: space-around;
    cursor: pointer;
}

.filter-button {
    color: white;
    margin-left: 10px;
    width: 100px;
    border: 1px solid white;
    border-radius: 2%;
    display: flex;
    justify-content: space-around;
    cursor: pointer;
}

.month-button-box {
    width: 220px;
    margin-left: 40px;
    display: flex;
    justify-content: space-around;
    align-items: center;
}

.month-button-box span {
    color: white;
}

.month-button-value {
    display: flex;
    flex-direction: row;
    width: 120px;
    justify-content: space-between;
    align-items: center
}

span {
    cursor: pointer;
}

.switchBox {
    padding-left: 20px;
    display: flex;
    flex-direction: row;
    justify-content: space-between;
    align-items: center
}

.switchBox span {
    font-size: 10px;
    color: white;
}

input[type="checkbox"] {
    position: relative;
    width: 50px;
    height: 10px;
    -webkit-appearance: none;
    outline: none;
    background: #c6c6c6;
    border-radius: 20px;
    transition: .5s;
    box-shadow: inset 0 0 5px rgba(0, 0, 0, .2);
}

input[type="checkbox"]:before {
    content: '';
    position: absolute;
    width: 10px;
    height: 10px;
    border-radius: 20px;
    top: 0;
    left: 0;
    background: #fff;
    transition: .5s;
    transform: scale(1.1);
    box-shadow: 0 2px 5px rgba(0, 0, 0, .2);
}

input:checked[type="checkbox"]:before {
    left: 40px;
}

i {
    color: white;
    font-size: 20px
}
</style>