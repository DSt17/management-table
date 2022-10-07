<template>
    <div class="calendar_popup_wrapper" ref="calendar_popup_wrapper">
	  <div class="v_popup">
		<div class="popup_header">
		    <span style="padding-left: 5px">Add range date.</span>
		    <span>
			  <i style="font-size: 18px" class="material-icons" v-on:click="closeCalendarPopup">cancel</i>
		    </span>
		</div>
		
		<div class="calendar_popup_content">
		    <form v-on:submit.prevent="onSubmit">
			  <div style="display: flex; flex-direction: column">
				<span v-if="new Date(this.dayStart).getDay() === 6 || new Date(this.dayStart).getDay() === 0 ||
						new Date(this.dayFinish).getDay() === 6 || new Date(this.dayFinish).getDay() === 0"
					style="color:#ff6e6e ; font-size: 12px; text-shadow:black 0 2px 5px"
				>
						  Don't take a day off!!!
					  </span>
				<span v-else style="color:white ; font-size: 12px">
				    don't take a day off in calendar..
				</span>
				
				
				<div style="display: flex; flex-direction: row;justify-content: space-between; height: 35px">
				    <span>Day start: </span>
				    <input
					    :class="new Date(this.dayStart).getDay() === 6 ||
					     new Date(this.dayStart).getDay() === 0  ? 'incorrectValue' : '' "
					    type="date"
					    v-model="dayStart"
				    >
				</div>
				<div style="display: flex; flex-direction: row;justify-content: space-between; height: 35px">
				    <span>Day finish: </span>
				    <input
					    :class="new Date(this.dayFinish).getDay() === 6 ||
					     new Date(this.dayFinish).getDay() === 0 ? 'incorrectValue' : '' "
					    type="date"
					    v-model="dayFinish"
				    >
				</div>
				
				<button ref="buttonPopup"
					  type="submit"
					  :disabled="isDisabled"
					  @click="onSubmit"
				>Apply
				</button>
			  </div>
		    </form>
		</div>
	  </div>
    </div>
</template>

<script>


export default {
    name: "CalendarPopup",
    data() {
	  return {
		dayStart: '',
		dayFinish: '',
		dayStartParse: '',
		dayEndParse: '',
		totalDays: '',
		rangeDayOfTheWeekNumberArray: [],
		clickedCalendarMonths: false
	  }
    },
    methods: {
	  closeCalendarPopup() {
		this.$emit('closeCalendarPopup')
	  },
	  onSubmit() {
		this.dayStartParse = Date.parse(this.dayStart)
		this.dayEndParse = Date.parse(this.dayFinish)
		this.totalDays = ((this.dayEndParse - this.dayStartParse) / 86400000) + 1
		for (let i = this.dayStartParse; i <= this.dayEndParse; i = i + 86400000) {
		    this.rangeDayOfTheWeekNumberArray.push(new Date(i).toLocaleDateString())
		}
		this.clickedCalendarMonths = true
		this.$emit('clickedCalendarMonths', this.clickedCalendarMonths)
		this.$emit('deyStart', this.dayStart)
		this.$emit('dayFinish', this.dayFinish)
		this.$emit('rangeDayOfTheWeekNumberArray', this.rangeDayOfTheWeekNumberArray)
	  },
    },
    mounted() {
	  let vm = this;
	  document.addEventListener('click', function (item) {
		if (item.target === vm.$refs['calendar_popup_wrapper'] || item.target === vm.$refs['buttonPopup']) {
		    vm.closeCalendarPopup()
		}
	  })
    },
    computed: {
	  isDisabled() {
		return !(new Date(this.dayStart).getDay() !== 6 &&
			new Date(this.dayStart).getDay() !== 0 &&
			new Date(this.dayFinish).getDay() !== 6 &&
			new Date(this.dayFinish).getDay() !== 0 &&
			this.dayStart.trim() && this.dayFinish.trim()) ? true : false;
	  },
    }
}

</script>

<style scoped>
.v_popup {
    color: white;
    position: fixed;
    top: 100px;
    left: 803px;
    width: 190px;
    background: #346977;
    box-shadow: 0 8px 17px 0 rgba(0, 0, 0, 0.2);
    border: 1px solid white;
    z-index: 1;
}

.popup_header {
    display: flex;
    justify-content: space-between;
    align-items: center;
}

.calendar_popup_content {
    display: flex;
    justify-content: center;
    align-items: center;
}

.calendar_popup_wrapper {
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

form div {
    display: flex;
    align-items: center;
    flex-direction: row;
    justify-content: space-between
}

form input {
    width: 120px;
}

form button {
    margin-top: 5px;
    border-color: white;
    width: 50px;
    margin-bottom: 2px;
}

span {
    font-size: 10px;
}

.incorrectValue {
    border: 2px solid red;
    outline: none;
}
</style>