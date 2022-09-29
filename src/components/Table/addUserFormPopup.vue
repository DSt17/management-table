<template>
    <div class="popup_wrapper" ref="popup_wrapper">
	  <div class="v_popup">
		<div class="popup_header">
		    <span>Add new person..</span>
		    <span>
			  <i class="material-icons" v-on:click="closePopup">cancel</i>
		    </span>
		</div>
		
		<div class="popup_content">
		    <form v-on:submit.prevent="onSubmit">
			  <div>
				<p>Name</p>
				<input type="text"
					 v-model="newUserName"
				>
				<button type="submit"
					  @click="onSubmit"
				>Add
				</button>
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
	  }
    },
    methods: {
	  closePopup() {
		this.$emit('closePopup')
	  },
	  onSubmit() {
		if (this.newUserName.trim() !== '') {
		    const newUser = {
			  id: new Date().getTime(), title: this.newUserName,
			  status: 'Free',
			  projects: []
		    }
		    this.$emit('AddNewUser', newUser)
		    this.newUserName = ''
		}
	  }
    },
    mounted() {
	  let vm = this;
	  document.addEventListener('click', function (item) {
		if (item.target === vm.$refs['popup_wrapper']) {
		    vm.closePopup()
		}
	  })
    },
}
</script>

<style scoped>

.v_popup {
    color: white;
    padding: 16px;
    position: fixed;
    top: 150px;
    left: 350px;
    width: 400px;
    background: #346977;
    box-shadow: 0 8px 17px 0 rgba(0, 0, 0, 0.2);
}

.popup_header {
    display: flex;
    justify-content: space-between;
    align-items: center;
}

.popup_content {
    display: flex;
    justify-content: center;
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

form div {
    display: flex;
    align-items: center;
    flex-direction: row;
    justify-content: space-between
}

form input {
    width: 300px;
    margin-left: 20px;
    margin-right: 10px
}

form button {
    border-color: white
}
</style>