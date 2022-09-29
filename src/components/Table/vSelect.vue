<template>
    <div class="v-select">
	  <p class="title"
	     @click="areOptionsWisible = !areOptionsWisible"
	  >
		{{ selected }}
	  </p>
	  <div class="option" v-if="areOptionsWisible">
		<p
			v-for="option in options"
			:key="option.value"
			@click="selectOption(option)"
		>
		    {{ option.name }}
		</p>
	  </div>
    </div>
</template>

<script>
export default {
    name: "vSelect",
    props: {
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
		areOptionsWisible: false
	  }
    },
    methods: {
	  selectOption(option) {
		this.$emit('select', option)
		this.areOptionsWisible = false
	  },
    },
}
</script>

<style scoped>

.v-select {
    position: relative;
    width: 90px;
    cursor: pointer;
    color: white;
    background-color: #346977;
}

.v-select p {
    margin: 0;
}

.option {
    border: 1px solid white;
    position: absolute;
    top: 30px;
    right: 0;
    width: 100%;
    color: white;
    background-color: #346977;
}

.option p:hover {
    border: 1px solid white;
}
</style>