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
					  <select v-model="selectedEmployee">
						<option v-for="(employee,idx) in employee"
							  :key="idx"
						>
						    {{ employee }}
						</option>
					  </select>
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
		employeeName: '',
		selectedEmployee: '',
		dayStart: '',
		dayFinish: '',
		employee: ["Katrin Flanders", "Jon Smith", "Jet Li ", "Katya Bloom", "Chack Jones","Pavel Poddubniy","Poul Blond","Jeck Loony ","Elizavetta Cruz"]
	  }
    },
    methods: {
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
		    this.$emit('AddNewTaskInCompany', newTask, this.item.title)
		    this.selectedEmployee = ''
		    this.employeeName = ''
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
	  },
    }
    
}
</script>

<style scoped>
.v_popup {
    color: white;
    padding: 2px;
    position: fixed;
    top: 137px;
    left: 179px;
    width: 205px;
    height: 260px;
    background: #346977;
    box-shadow: 0 8px 17px 0 rgba(0, 0, 0, 0.2);
    border: 1px solid white;
    border-radius: 5px;
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
    margin-top: 20px;
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
</style>