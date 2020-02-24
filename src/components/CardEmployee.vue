<template>
	<div>
		<div class="card-employee" v-for="(dk, index) in data" :key="index">
			<img :src="dk.avatar_url" alt="Foto Profil" width="100%">
			<div class="container">
				<div class="address">{{ dk.addresses[0].city }}</div>
				<div class="name">{{ dk.first_name}}</div>
				<p class="absence" v-if="dk.absence_okt2019">{{ dk.absence_okt2019 }} days absence in Oktober 2019</p>
				<p class="salary" v-else>{{ getSalary(dk.salary) }}</p>
			</div>
		</div>
	</div>
</template>

<script>
export default {
	name: 'CardEmployee',
	props: {
		data: Array
	},
	data () {
		return {

		}
	},
	methods: {
		getSalary(val) {
			val = val.toString()
			let number = val
			// const newVal = val.substring(0, val.length - 3) 
			// const newVal = val.toString()

			const decPlaces = Math.pow(10, 3)

			const abbrev = ["K", "M", "B", "T"]

			for (var i = abbrev.length - 1; i >= 0; i--) {
				var size = Math.pow(10, (i + 1)*3)

				if (size <= number) {
					number = Math.round(number*decPlaces/size)/decPlaces

					if ((number == 1000) && (i < abbrev.length -1)) {
						number = 1
						i++
					} 

					// Add the letter for the abbreviation
					number += abbrev[i];

					// We are done... stop
					break
				} 
			} 


			console.log(decPlaces)
			return 'IDR '+ number
		}
	}
}
</script>

<style lang="scss" scoped>
	.card-employee {
		width: 20%;
		border-radius: 20px;
		box-shadow: 0 4px 8px 0 rgba(0,0,0,0.2);
		display: inline-block;
		margin-right: 25px;
	}

	.card-employee {
		img {
			border-radius: 20px 20px 0px 0px;
		}

		.container {
			padding: 10px;
		}
		
		.name {
			font-size: 16px;
			font-weight: bold;
			margin-top: 5px;
		}
		.address {
			font-size: 10px;
			color: gray;
			line-height: 10px;
		}
		.salary {
			text-align: right;
			color: #858c93;
			font-weight: bold;
			font-size: 13px;
		}
		.absence {
			font-size: 10px;
			font-weight: 700;
			color: #7d7d7d;
		}
	}

</style>