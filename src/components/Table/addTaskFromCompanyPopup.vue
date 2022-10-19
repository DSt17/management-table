<template>
    <div class="popup_wrapper" ref="popup_wrapper">
	  <div class="v_popup">
		<div class="popup_header">
		    <p style="font-size: 18px;margin-left: 10px">{{ item.title }}</p>
		    <span>
			  <i class="material-icons" v-on:click="closeAddTaskPopup">cancel</i>
		    </span>
		</div>
		<span v-if="new Date(this.dayStart).getDay() === 6 || new Date(this.dayStart).getDay() === 0 ||
						new Date(this.dayFinish).getDay() === 6 || new Date(this.dayFinish).getDay() === 0"
			style="color:#ff6e6e ; font-size: 14px;text-shadow:black 0 2px 5px"
		>
						  Don't take a day off!!!
					  </span>
		<span v-else style="color:white ; font-size: 12px">
				    don't take a day off in calendar..
				</span>
		<div>
		    <form v-on:submit.prevent="onSubmit">
			  
			  <div style="display: flex; flex-direction: column; padding: 5px">
				<div class="formBody">
				    <div>
					  <span>Project:</span>
					  <input type="text"
						   v-model="projectName"
						   placeholder="project name.."
						   title="value must be a string!"
						   pattern="^[a-zA-Z\s]+$"
					  >
				    </div>
				    <div>
					  <span>Task:</span>
					  <input type="text"
						   v-model="employeeName"
						   placeholder="new task title.."
						   title="value must be a string!"
						   pattern="^[a-zA-Z\s]+$"
					  >
				    </div>
				    <div>
					  <span>Employee:</span>
					  <input type="text"
						   id="checkedInput"
						   @change="checkInput"
						   list="employmentsArray"
						   placeholder="select or create.."
						   title="value must be a string!"
						   pattern="^[a-zA-Z\s]+$"
						   autocomplete="off"
					  >
					  <datalist id="employmentsArray">
						<option v-for="employee in employees">{{ employee }}</option>
					  </datalist>
				    
				    </div>
				    <div>
					  <span>Day start:</span>
					  <input
						  :class="new Date(this.dayStart).getDay() === 6 ||
					     new Date(this.dayStart).getDay() === 0  ? 'incorrectValue' : '' "
						  type="date"
						  v-model="dayStart"
					  >
				    </div>
				    <div>
					  <span>Day finish:</span>
					  <input
						  :class="new Date(this.dayFinish).getDay() === 6 ||
					     new Date(this.dayFinish).getDay() === 0 ? 'incorrectValue' : '' "
						  type="date"
						  v-model="dayFinish"
					  >
				    </div>
				    <div class="range-slider">
					  <span>Chance:</span>
					  <span>{{rangeChance}} %</span>
					  <input class="chanceSlider"
						   id="rangeChance" type="range" min="25" max="100" step="25"
						   v-model.number="rangeChance"
						   @change = "changeChanceRangeColor"
					  >
				    </div>
				    <div>
					  <button type="submit"
						    @click="onSubmit"
						    :disabled="isDisabled"
					  >
						Add
					  </button>
				    </div>
				</div>
			  </div>
		    </form>
		</div>
	  </div>
    </div>
</template>
<script>
export default {
    name: "addTaskFromCompanyPopup",
    props: ["item"],
    data() {
	  return {
		projectName:'',
		employeeName: '',
		selectedEmployee: '',
		dayStart: '',
		dayFinish: '',
		rangeChance:100,
		employees: ["Katrin Flanders", "Jon Smith", "Jet Li ", "Katya Bloom", "Chack Jones", "Pavel Poddubniy", "Poul Blond", "Jeck Loony ", "Elizavetta Cruz"]
	  }
    },
    methods: {
	  changeChanceRangeColor(){
		const rangeLine = document.getElementById('rangeChance')
		if(this.rangeChance <= 25){
		    rangeLine.style.background = '#f51d2d'
		}
		if(this.rangeChance === 50){
		    rangeLine.style.background = 'orange'
		}
		if(this.rangeChance === 75){
		    rangeLine.style.background = 'yellow'
		}
		if(this.rangeChance === 100){
		    rangeLine.style.background = '#00d90087'
		}
	  },
	  checkInput(event) {
		this.selectedEmployee = event.target.value
	  },
	  closeAddTaskPopup() {
		this.$emit('closeAddTaskPopup')
	  },
	  onSubmit() {
		if (this.employeeName.trim() !== '' && this.selectedEmployee.trim() !== ''
			&& this.dayStart.trim() !== '' && this.dayFinish.trim() !== '') {
		    const newTask = {
			  task: this.employeeName,
			  customer: this.selectedEmployee,
			  workingTimeHours: 5,
			  dayStart: this.dayStart.replace(/-/g, '/'),
			  dayFinish: this.dayFinish.replace(/-/g, '/')
		    }
		    const newUser = {
			  id: new Date().getTime(),
			  title: this.selectedEmployee,
			  status: 'in Process',
			  vacation: [],
			  sickDay: [],
			  projects: [
				{
				    projectName:this.projectName,
				    task: this.employeeName,
				    customer: this.item.title,
				    workingTimeHours: 5,
				    dayStart: this.dayStart.replace(/-/g, '/'),
				    dayFinish: this.dayFinish.replace(/-/g, '/')
				},
			  ]
		    }
		    this.$emit('AddNewTaskInCompany', newUser, newTask)
		    this.selectedEmployee = ''
		    this.employeeName = ''
		    this.projectName = ''
		    this.dayStart = ''
		    this.dayFinish = ''
		    document.getElementById('checkedInput').value = ''
		}
	  },
    },
    mounted() {
	  let vm = this;
	  document.addEventListener('click', function (item) {
		if (item.target === vm.$refs['popup_wrapper']) {
		    vm.closeAddTaskPopup()
		}
	  })
    },
    computed: {
	  isDisabled() {
		return this.employeeName.trim() !== '' && !Number(this.employeeName)
		&& this.selectedEmployee.trim() !== '' && this.dayStart.trim() !== ''
		&& this.dayFinish.trim() !== '' && new Date(this.dayStart).getDay() !== 6
		&& new Date(this.dayStart).getDay() !== 0 && new Date(this.dayFinish).getDay() !== 6
		&& new Date(this.dayFinish).getDay() !== 0 ? false : true;
	  }
    }
}
</script>

<style scoped>
.v_popup {
    color: white;
    padding: 2px;
    position: fixed;
    top: 108px;
    left: 179px;
    width: 210px;
    height: 301px;
    background: #346977;
    box-shadow: 0 8px 17px 0 rgba(0, 0, 0, 0.2);
    border: 1px solid white;
    border-radius: 5px;
    z-index: 3;
}

.popup_header {
    display: flex;
    justify-content: space-between;
    align-items: center;
}

.popup_wrapper {
    display: flex;
    justify-content: center;
    align-items: center;
    position: absolute;
    right: 0;
    left: 0;
    top: 0;
    bottom: 0;
}

form {
    width: 100%;
    height: 100%;
}

.formBody div {
    display: flex;
    flex-direction: row;
    justify-content: space-between;
    padding-top: 10px;
}

button {
    border-color: white;
    width: 80px;
    height: 20px;
}

input:focus:invalid {
    border: 2px solid red;
    color: red;
}

input {
    outline: none;
    width: 100px;
}

.incorrectValue {
    border: 2px solid red;
}

::placeholder {
    font-size: 10px;
}
span{
    font-size: 12px;
}
.chanceSlider {
    -webkit-appearance: none;
    width: 105px;
    height: 4px;
    border-radius: 5px;
    background: #00d90087;
    outline: none;
    margin: 5px 0 10px 0;
}

.chanceSlider::-webkit-slider-thumb {
    appearance: none;
    width: 11px;
    height: 11px;
    border-radius: 100%;
    background: white;
    cursor: pointer;
}
</style>