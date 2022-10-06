<template>
    <div>
	  <addUserFormPopup
		  v-if="isInfoPopupVisible"
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
		  v-if="taskPopupVisible"
		  @closeAddTaskPopup="closeAddTaskPopup"
		  @AddNewTask="AddNewTask"
		  :item="item"
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
				<input id="switchBoxToggle" type="checkbox">
			  </div>
			  <span @click="changeToggleTrue">Company</span>
		    </div>
		    <div class="sort-button"
			   @click="sortedByName">
			  <span>sort </span>
			  <i style="font-size: 20px" class="material-icons">sort_by_alpha</i>
		    </div>
		    <div class="filter-button">
			  <vSelect
				  :options="options"
				  @select="optionSelect"
				  :selected="selected"
			  />
			  <i style="font-size: 20px;color:white" class="material-icons">filter_alt</i>
		    </div>
		    <div class="add-button"
			   @click="showPopupInfo"
		    >
			  <span>Add new task </span>
			  <i style="color: white; font-size: 20px" class="material-icons">person_add</i>
		    </div>
		    
		    <div class="month-button" style="color: white">
			  <span @click="prevMonth"><<</span>
			  <div style="display: flex; flex-direction: row; width: 120px; justify-content: space-between; align-items: center">
				<div v-if="clickedCalendar === false">{{ currentMonth }}</div>
				<div v-if="clickedCalendar === true" style="font-size: 10px">{{ dayStart.replace(/-/g, '/') }} -
				    {{ dayFinish.replace(/-/g, '/') }}
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

export default {
    name: "TableHeader",
    props: ["options", "selected", "taskPopupVisible", "item"],
    data() {
	  
	  return {
		isInfoPopupVisible: false,
		isCalendarPopupVisible: false,
		clickedCalendar: false,
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
	  AddNewUser(newUser) {
		this.$emit('AddNewUser', newUser)
	  },
	  AddNewTask(newTask, UserId) {
		this.$emit('AddNewTask', newTask, UserId)
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
		document.getElementById('switchBoxToggle').checked = false
		
	  },
	  changeToggleTrue() {
		document.getElementById('switchBoxToggle').checked = true
	  }
    },
    watch: {
	  filteredValue(filteredValue) {
		this.$emit('filteredValue', filteredValue)
	  }
    },
    components: {vSelect, addUserFormPopup, CalendarPopup, addTaskPopup}
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
    width: 100%;
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

.month-button {
    width: 220px;
    margin-left: 40px;
    display: flex;
    justify-content: space-around;
    align-items: center;
}

.month-button span {
    color: white;
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
</style>