<template>
    <div class="popup_wrapper" ref="popup_wrapper">
	  <div class="v_popup">
		<div class="popup_header">
		    <p style="font-size: 18px;margin-left: 10px">{{ item.title }}</p>
		    <span>
			  <i class="material-icons" v-on:click="closeAddTaskCalendarPopup">cancel</i>
		    </span>
		</div>
		
		
		<div :class="sickDayVisible === false ? 'tab-other' : 'tab-task'">
		    <div style="width: 50%; height: 100%; border-right: 1px solid white; cursor: pointer"
			   @click="sickDayVisible = false"
		    >
			  Task
		    </div>
		    <div style="width: 50%;height: 100%; background-color: rgb(88 76 76); cursor: pointer"
			   @click="sickDayVisible = true"
		    >
			  Other..
		    </div>
		</div>
		<div v-if="sickDayVisible === true"
		     style="width: 100%; height: 172px; border-bottom-left-radius: 5px;
		     border-bottom-right-radius: 5px;background-color: rgb(88 76 76);"
		>
		    <div style="display: flex; flex-direction: column; padding: 5px">
			  <div class="formBody">
				<div>
				    <span>Sick day:</span>
				    <input type="radio" id="sickDay" value="sick day" v-model="picked">
				</div>
				<div style="margin-top: 3px">
				    <span>Vacation:</span>
				    <input type="radio" id="vacation" value="vacation" v-model="picked">
				
				</div>
				<div>
				    <span>Day start:</span>
				    <span>{{ ArraySelectedDays[0] }}</span>
				</div>
				<div>
				    <span>Day finish:</span>
				    <span>{{ ArraySelectedDays[ArraySelectedDays.length - 1] }}</span>
				</div>
				<div>
				    <button
					    @click="setVacationAndStickDay"
					    :disabled="isDisabledOtherTabButton"
				    >
					  Add
				    </button>
				    <div>{{ picked }}</div>
				</div>
			  </div>
		    </div>
		</div>
		<div v-if="sickDayVisible === false">
		    <form id="onSubmit" v-on:submit.prevent="onSubmit">
			  <div style="display: flex; flex-direction: column; padding: 5px">
				<div class="formBody">
				    <div>
					  <span>Task:</span>
					  <input type="text"
						   style=" width: 100px; outline: none;"
						   v-model="taskTitle"
						   placeholder="new task title.."
						   title="value must be a string!"
						   pattern="^[a-zA-Z\s]+$"
					  >
				    </div>
				    <div>
					  <span>Customer:</span>
					  <span>{{ customer }}</span>
				    </div>
				    <div>
					  <span>Day start:</span>
					  <span>{{ ArraySelectedDays[0] }}</span>
				    </div>
				    <div>
					  <span>Day finish:</span>
					  <span>{{ ArraySelectedDays[ArraySelectedDays.length - 1] }}</span>
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
    name: "addTaskCalendarPopup",
    props: ["item", "ArraySelectedDays", "customer"],
    data() {
	  return {
		taskTitle: '',
		sickDayVisible: false,
		picked: ''
	  }
    },
    methods: {
	  closeAddTaskCalendarPopup() {
		this.$emit('closeAddTaskCalendarPopup')
	  },
	  onSubmit() {
		if (this.taskTitle.trim() !== '') {
		    const newTask = {
			  projectName: '',
			  task: this.taskTitle,
			  customer: this.customer,
			  workingTimeHours: 5,
			  dayStart: this.ArraySelectedDays[0],
			  dayFinish: this.ArraySelectedDays[this.ArraySelectedDays.length - 1]
		    }
		    this.$emit('AddNewTaskClickedCalendar', newTask, this.item.id)
		    this.taskTitle = ''
		    this.$emit('closeAddTaskCalendarPopup')
		}
	  },
	  setVacationAndStickDay() {
		this.$emit('addVacationOrStickDay', this.ArraySelectedDays, this.item.id, this.picked)
		this.$emit('closeAddTaskCalendarPopup')
	  }
    },
    mounted() {
	  let vm = this;
	  document.addEventListener('click', function (item) {
		if (item.target === vm.$refs['popup_wrapper']) {
		    vm.closeAddTaskCalendarPopup()
		}
	  })
    },
    computed: {
	  isDisabled() {
		return this.taskTitle.trim() !== '' && !Number(this.taskTitle) &&
		new Date(this.ArraySelectedDays[0]).getDay() !== 6 &&
		new Date(this.ArraySelectedDays[0]).getDay() !== 0 &&
		new Date(this.ArraySelectedDays[this.ArraySelectedDays.length - 1]).getDay() !== 6
		&& new Date(this.ArraySelectedDays[this.ArraySelectedDays.length - 1]).getDay() !== 0 ? false : true;
	  },
	  isDisabledOtherTabButton() {
		return this.picked !== '' ? false : true
	  }
    }
}
</script>

<style scoped>
.v_popup {
    color: white;
    position: fixed;
    top: 137px;
    left: 179px;
    width: 205px;
    height: 260px;
    background-color: #666060;
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
    margin-top: 15px;
}

input:focus:invalid {
    border: 2px solid red;
    color: red;
}


.incorrectValue {
    border: 2px solid red;
}

::placeholder {
    font-size: 10px;
}

.tab-other {
    height: 30px;
    display: flex;
    flex-direction: row;
    border-top: 1px solid white;
}

.tab-task {
    height: 30px;
    display: flex;
    flex-direction: row;
    border-top: 1px solid white;
}
</style>
