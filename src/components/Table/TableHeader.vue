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
	  
	  
	  <div class="table-header-navbar-box">
		<div class="header-navbar-navigation-box">
		    <div>
			  <input
				  type="text"
				  placeholder="Search..."
				  style="width: 250px;"
				  v-model="filteredValue"
			  >
		    
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
			  <span>Add new person </span>
			  <i style="color: white; font-size: 20px" class="material-icons">person_add</i>
		    </div>
		    
		    <div class="month-button" style="color: white">
			  <span @click="prevMonth"><<</span>
			  <div style="display: flex; flex-direction: row; width: 120px; justify-content: space-between;align-items: center">
				<div v-if="clickedCalendar === false">{{ currentMonth }}</div>
				<div v-if="clickedCalendar === true" style="font-size: 8px">{{ dayStart.replace(/-/g, '/') }} -
				    {{ dayFinish.replace(/-/g, '/') }}
				</div>
				<div><i @click="showCalendarPopupInfo" style="font-size: 20px; padding-top: 4px"
					  class="material-icons">calendar_month</i>
				</div>
			  </div>
			  <span @click="nextMonth">>></span>
		    </div>
		
		</div>
	  </div>
	  <div>
		<table>
		    <tr>
			  <td style="width: 150px">
				<div style="display: flex; flex-direction: row; justify-content: space-around">
				    <div style="border-right: 1px solid black;width: 50%">Name</div>
				    <div>Client</div>
				</div>
			  </td>
			  <td
				  v-for="(day,idx) in arrayForRendering()"
				  :key="idx"
				  :class="new Date(day).getDay() === 6 || new Date(day).getDay() === 0 ? 'weekendDay' : '' "
			  >{{ new Date(day).getDate() }}
			  </td>
		    </tr>
		</table>
	  </div>
    
    </div>

</template>

<script>
import vSelect from '@/components/Table/vSelect'
import addUserFormPopup from '@/components/Table/addUserFormPopup'
import CalendarPopup from '@/components/Table/CalendarPopup'

export default {
    name: "TableHeader",
    props: {
	  dayInAMonths: Array,
	  options: {
		type: Array,
		default: []
	  },
	  selected: {
		type: String,
		default: ''
	  }
    },
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
	  showCalendarPopupInfo() {
		this.isCalendarPopupVisible = true
	  },
	  closeCalendarPopup() {
		this.isCalendarPopupVisible = false
	  },
	  rangeDay(array) {
		this.rangeDayArray = array
		this.$emit('rangeDayArray', this.rangeDayArray)
	  },
	  prevMonth() {
		this.clickedCalendar = false
		this.$emit('clickedCalendar', this.clickedCalendar)
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
		this.$emit('clickedCalendar', this.clickedCalendar)
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
		this.$emit('clickedCalendar', this.clickedCalendar)
	  },
	  arrayForRendering() {
		if (this.clickedCalendar === true) {
		    return this.rangeDayArray
		} else {
		    return this.dayInAMonths
		}
	  }
    },
    watch: {
	  filteredValue(filteredValue) {
		this.$emit('filteredValue', filteredValue)
	  }
    },
    components: {vSelect, addUserFormPopup, CalendarPopup}
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
    width: 1020px;
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
}


.add-button {
    color: white;
    margin-left: 40px;
    width: 200px;
    border: 1px solid white;
    border-radius: 2%;
    display: flex;
    justify-content: space-around
}

.filter-button {
    color: white;
    margin-left: 10px;
    width: 120px;
    border: 1px solid white;
    border-radius: 2%;
    display: flex;
    justify-content: space-around
}

.month-button {
    width: 220px;
    margin-left: 70px;
    display: flex;
    justify-content: space-around;
    align-items: center;
}

.month-button span {
    color: white;
}

.weekendDay {
    background-color: #f3f2f2;
}


</style>