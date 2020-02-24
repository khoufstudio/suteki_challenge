<template>
	<div id="app">
		<div class="header">
			<a href="#" class="logo">SUTEKI CHALLENGE</a>			
		</div>

		<div class="body">
			<h1>Data Karyawan</h1>
			<!-- filter container -->
			<div class="container">
				<div class="sidebar">
					<div class="filter-container">
						<!-- <h2>Filter</h2> -->
						<div class="filter-title">Filter</div>
						<a :class="{ active: isActive(1) }" href="#" @click="fetchData(1)">Semua Karyawan</a> 
						<a :class="{ active: isActive(2) }" href="#" @click="fetchData(2)">Gaji di atas Rp 15.000.000</a> 
						<a :class="{ active: isActive(3) }" href="#" @click="fetchData(3)">Berdomisili di Jakarta</a> 
						<a :class="{ active: isActive(4) }" href="#" @click="fetchData(4)">Ulang tahun pada bulan Maret</a> 
						<a :class="{ active: isActive(5) }" href="#" @click="fetchData(5)">Berada di departemen Research and Development</a> 
						<a :class="{ active: isActive(6) }" href="#" @click="fetchData(6)">Jumlah hari tidak masuk dari setiap karyawan pada October 2019</a>
					</div>
				</div>

				<div class="content">
					<div v-if="loadingFlag">Loading...</div>
					<!-- list container -->
					<div class="list-container" v-if="!loadingFlag"> 
						<card-employee :data="dataKaryawan" v-if="dataKaryawan.length"></card-employee>
						<div v-else>Tidak ada data</div>
					</div>
				</div>
			</div>
		</div>
	</div>
</template>

<script>
import axios from 'axios'
import CardEmployee from "./components/CardEmployee.vue";

export default {
	name: 'App',
	components: {
		CardEmployee
	},
	data() {
		return {
			baseUrl: process.env.VUE_APP_BASE_URL,
			dataKaryawan: [],
			loadingFlag: false,
			panelStatus: [false, true, false, false, false, false, false, false],
			activePanel: 1
		}
	},
	methods: {
		fetchData(val) {
			axios.get(this.baseUrl + 'employee.json')
				.then(response => {
					let dataTemp = []
					this.dataKaryawan = []
					this.loadingFlag = true

					switch (val) {
						case 1: 
							dataTemp = response.data
							this.panelStatus[this.activePanel] = false
							this.activePanel = 1
							this.panelStatus[this.activePanel] = true
							break
						case 2:
							dataTemp = response.data.filter(dt => {
								return dt.salary > 15000000
							})
							this.panelStatus[this.activePanel] = false
							this.activePanel = 2
							this.panelStatus[this.activePanel] = true
							break
						case 3: 
							dataTemp = response.data.filter(dt => {
								return dt.addresses[0].city == 'DKI Jakarta'
							})
							this.panelStatus[this.activePanel] = false
							this.activePanel = 3
							this.panelStatus[this.activePanel] = true
							break
						case 4:
							dataTemp = response.data.filter(dt => {
								let birthday = new Date(dt.birthday)
								let month = birthday.getMonth()+1
								if (month == 3) {
									return dt
								} 
							})
							this.panelStatus[this.activePanel] = false
							this.activePanel = 4
							this.panelStatus[this.activePanel] = true
							break
						case 5:
							dataTemp = response.data.filter(dt => {
								return dt.department.name == "Research and development"
							}) 
							this.panelStatus[this.activePanel] = false
							this.activePanel = 5
							this.panelStatus[this.activePanel] = true
							break
						case 6: 
							dataTemp = response.data
							response.data.filter(dt => {
								let absenceOkt = []
								absenceOkt = dt.presence_list.filter(abs => {
									let absence = new Date(abs)
									if (absence.getMonth()+1 == 10) {
										return abs
									} 
								})

								console.log(31-absenceOkt.length)

								dt.absence_okt2019 = 31 - absenceOkt.length
								this.panelStatus[this.activePanel] = false
								this.activePanel = 6
								this.panelStatus[this.activePanel] = true
								return dt
							})
							break
					}

					setTimeout(() => {
						this.dataKaryawan = dataTemp
						this.loadingFlag = false
					}, 1000)

				})
		},
		isActive(val) {
			return this.panelStatus[val]
		}
	},
	created() {
		this.fetchData(1)
	}
}
</script>

<style lang="scss" scoped>
	@import url('https://fonts.googleapis.com/css?family=Montserrat&display=swap');

	$colorPrimary: #6200EE;
	
	body, html {
		font-family: 'Montserrat', sans-serif;
	}

	.container {
		padding: 0px;
		position: relative;

	}

	#app {
		font-family: 'Montserrat', sans-serif;
		padding: 0px;
		margin: 0px;
		height: 100vh;
		width: 100vw;
	}

	.sidebar {
		width: 30%;
		/*width: 300px;*/
		display: inline-block;
		/*background-color: blue;*/
		/*position: absolute;*/
		/* top: 0;
		left: 0; */
	}

	.content {
		position: absolute;
		top: 0;
		right: 0;
		width: 70%;
		display: inline-block;
		/*background-color: black;*/
	}

	.filter-container {
		border: 1px solid #d3d3d3;
		border-radius: 10px;
		margin-right: 25px;
		box-shadow: 0 2px 5px 0 rgba(0,0,0,0.2);

	}

	.filter-container {
		.filter-title {
			padding: 10px 10px;
			font-size: 18px;
			font-weight: 700;
		}

		a {
			font-size: 13px;
			display: block;
			padding: 10px;
			text-decoration: none;
			border-bottom: 1px solid #d3d3d3;
			color: black;
		}

		a:hover, .active {
			background-color: #ccc;		
			font-weight: bold;
		}

		a:nth-child(2) {
			border-top: 1px solid #d3d3d3;
		}
		
		a:nth-last-child(1) {
			border-bottom: 0px;
		}
	}


	.list-container {
		/*background-color: black;*/
		width: 100%;
	}

	.header {
		background-color: $colorPrimary;
		padding: 25px;
		margin: 0px;
		/*border-bottom: 3px solid #f8f9fa;*/
		color: white;
		box-shadow: 0 2px 5px 0 rgba(0,0,0,0.2);
	}

	.header .logo {
		font-weight: 800;
		text-decoration: none;
		color: white;
		font-size: 25px;
	}

	.body {
		padding: 0px 25px;
	}

</style>
