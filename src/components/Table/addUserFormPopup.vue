<template>
    <div class="popup_wrapper" ref="popup_wrapper">
	  <div class="v_popup">
		<div class="popup_header">
		    <p style="font-size: 12px;margin-left: 10px">Fill out the form (all fields are required)</p>
		    <span>
			  <i class="material-icons" v-on:click="closePopup">cancel</i>
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
					  <span>Name:</span>
					  <input type="text"
						   id="checkedInput"
						   @change="checkInput"
						   list="employmentsArray"
						   placeholder="select or create employee.."
						   title="value must be a string!"
						   pattern="^[a-zA-Z\s]+$"
						   autocomplete="off"
					  >
					  <datalist id="employmentsArray">
						<option v-for="employee in employees">{{ employee }}</option>
					  </datalist>
				    </div>
				    <div>
					  <span>Task:</span>
					  <input type="text"
						   v-model="taskTitle"
						   placeholder="new task title.."
						   title="value must be a string!"
						   pattern="^[a-zA-Z\s]+$"
					  >
				    </div>
				    <div>
					  <span>Customer:</span>
					  <select v-model="selectedCustomer">
						<option v-for="(customer,idx) in customers"
							  :key="idx"
						>
						    {{ customer }}
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
		    </form>
		</div>
	  </div>
    </div>
</template>


<script>
export default {
    name: "addUserFormPopup",
    data() {
	  return {
		newUserName: '',
		taskTitle: '',
		selectedCustomer: '',
		dayStart: '',
		dayFinish: '',
		customers: ['FuryLion', 'Google', 'IBM', 'Microsoft', 'Yandex'],
		employees: ["Katrin Flanders", "Jon Smith", "Jet Li ", "Katya Bloom", "Chack Jones", "Pavel Poddubniy", "Poul Blond", "Jeck Loony ", "Elizavetta Cruz", "Alex Flax", "Emma Boost", "Jeelly Parse ", "Nikita Blade", "Max Stove", "Dave Frol", "Sofia Broot"]
		
	  }
    },
    methods: {
	  checkInput(event) {
		this.newUserName = event.target.value
	  },
	  closePopup() {
		this.$emit('closePopup')
	  },
	  onSubmit() {
		if (this.newUserName.trim() !== '' && this.taskTitle.trim() !== ''
			&& this.selectedCustomer.trim() !== '' && this.dayStart.trim() !== ''
			&& this.dayFinish.trim() !== '') {
		    const newUser = {
			  id: new Date().getTime(),
			  title: this.newUserName,
			  status: 'in Process',
			  vacation: [],
			  sickDay: [],
			  projects: [
				{
				    projectName: '',
				    task: this.taskTitle,
				    customer: this.selectedCustomer,
				    workingTimeHours: 5,
				    dayStart: this.dayStart.replace(/-/g, '/'),
				    dayFinish: this.dayFinish.replace(/-/g, '/')
				}
			  ]
		    }
		    this.$emit('AddNewUser', newUser)
		    this.newUserName = ''
		    this.selectedCustomer = ''
		    this.taskTitle = ''
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
		    vm.closePopup()
		}
	  })
    },
    computed: {
	  isDisabled() {
		return this.newUserName.trim() !== '' && !Number(this.newUserName)
		&& this.taskTitle.trim() !== '' && !Number(this.taskTitle)
		&& this.selectedCustomer.trim() !== '' && this.dayStart.trim() !== ''
		&& this.dayFinish.trim() !== '' && new Date(this.dayStart).getDay() !== 6 &&
		new Date(this.dayStart).getDay() !== 0 &&
		new Date(this.dayFinish).getDay() !== 6 &&
		new Date(this.dayFinish).getDay() !== 0 ? false : true;
	  },
    }
}
</script>

<style scoped>


.v_popup {
    color: white;
    padding: 2px;
    position: fixed;
    top: 120px;
    left: 555px;
    width: 250px;
    height: 265px;
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
    margin-top: 20px;
}

input:focus:invalid {
    border: 2px solid red;
    color: red;
}

input {
    outline: none;
    width: 150px;
}

::placeholder {
    font-size: 10px;
}

.incorrectValue {
    border: 2px solid red;
}
</style>