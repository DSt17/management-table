<template>
    <div>
	  <addUserFormPopup
		  v-if="isInfoPopupVisible"
		  @closePopup="closePopup"
		  @AddNewUser="AddNewUser"
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
		    <div class="month-button">
			  <span><<</span>
			  <i style="font-size: 20px" class="material-icons">calendar_month</i>
			  <span>>></span>
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
				  v-for="(day,idx) in dayInAMonths"
				  :key="idx"
			  >{{ day }}
			  </td>
		    </tr>
		</table>
	  </div>
    
    </div>

</template>

<script>
import vSelect from '@/components/Table/vSelect'
import addUserFormPopup from '@/components/Table/addUserFormPopup'

export default {
    name: "TableHeader",
    props: {
	  dayInAMonths: Number,
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
		filteredValue: '',
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
	  }
    },
    watch: {
	  filteredValue(filteredValue){
		this.$emit('filteredValue', filteredValue)
	  }
    },
    components: {vSelect, addUserFormPopup}
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
    width: 160px;
    margin-left: 100px;
    display: flex;
    justify-content: space-around;
}

.month-button span {
    color: white;
}

.month-button i {
    color: white;
    font-size: 17px;
    margin-left: 30px;
    margin-right: 30px;
}

</style>