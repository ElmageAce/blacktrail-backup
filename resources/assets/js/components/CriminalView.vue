<template>
	<div class="md:w-1/2 mr-6 font-basic" id="criminal_Page">
		<section>
			<p class="font-basic tracking-normal text-2xl mb-4 mt-4 font-normal text-black mr-2">
				Criminal Profile of {{ criminals.full_name }}
			</p>	
			<div class="bg-white px-8 py-8 pt-4 shadow-md">
				<div class="text-center">
					<div id="avatar" class="inline-block mb-6 w-full" >
						<img :src="avatarPath" class="h-50 w-50 rounded-full border-orange border-2">
						<p class="font-bold font-display mt-2 text-black text-3xl">{{ criminals.full_name }}</p>
						<p class="font-bold mt-2 text-orange text-2xl" v-text="criminalBounty === null ? 'No Bounty' : criminalBounty "></p>
						<!-- <crimes-list :criminals="crimes"></crimes-list> -->

						<div v-if="this.criminals.crimes.length > 0">
							<p class="mt-2 font-bold text-2xl">Notable Crimes:
							</p>
							<!-- @foreach ($criminal->crimes as $crime)  -->
							<!-- Crimes List -->

				<!-- <div class="mt-2 text-lg font-normal" v-if="criminals.crimes.length > 0 " v-for="criminal in criminals.crimes">
				<p class="font-bold text-md" v-text="">{{  criminal.criminal_offense }} - {{  criminal.pivot.crime_details }}</p>
				</div>
			-->

			<div id="crimesList">
				<div class="mt-2 text-lg font-normal" v-if="criminals.crimes.length > 0 " v-for="criminal in criminals.crimes">
					<p class="font-bold text-md" v-text="">{{  criminal.criminal_offense }} - {{  criminal.pivot.crime_description }}</p>
				</div>
			</div>
			<!-- <crimes-list :crimes="crimes" :criminals="criminals"></crimes-list> -->
		</div>
		<div v-else class="font-bold text-3xl font-basic mt-2 text-black-v2">
			No Crimes were listed for this criminal yet.
		</div>
		<!-- soon use slots here named or scoped  -->
		<div v-show="userRole === 1 || userRole === 2">
			<admin-buttons :id="criminalId" :criminals="criminals"></admin-buttons>
		</div>
		<div v-show="normalUser">
			<user-buttons :id="criminalId" :criminals="criminals"></user-buttons>
		</div>
	</div>
</div>
</div>
</section>
<div v-show="this.criminals === null">
	<p>No Criminals Profile</p>
</div>
</div>
</template>
<script>
import CrimesList from './CrimesList.vue';
import urlDomain from './scripts/endpoints.js';
import { publicPath, user, app } from './scripts/api.js';
import ViewActionLayout from './ViewActionLayout.vue' ;
import AdminButtons from './AdminButton.vue' ;
import UserButtons from './UserButton.vue' ;
import ChatBox from './modals/ChatBox.vue';
import _ from 'lodash';
export default {
	props: ['criminals','criminalId'], 	
	name: 'CriminalView',
	components : { 
		AdminButtons,
		ChatBox,
		UserButtons
	// CrimesList, 
},
data(){
	return {
			// criminalDetails : this.criminals,
		// criminalId :  $route.params.id ,
		// crimes : this.criminals.crimes
	}
},
methods : {
	showMorePhotos(){

	},

	fetchCrimes(){
		// we have criminal_id here. -> 3
		axios.get(this.fetchCriminalsInfoEndpoint)
		.then(response => { console.log(response); })
		.catch(error => { console.log(error); });
	},

	offerBounty(){
		console.log("Offering Bounty");
	},

	showMap(){
		this.$modal.show('location-map') ; 
	},

	reportCriminalLocation(){
		this.$modal.show('report-criminal');
	},

	check_if_the_currently_logged_on_user_is_the_creator(){
		console.log("check_if_the_currently_logged_on_user_is_the_creator... starts here.");
	}

},
mounted(){
	this.criminalDetails = this.criminals ; 
},


/*beforeRouteUpdate (to, from, next) {
	console.log("before route update");

},

beforeRouteEnter (to, from, next) {
	console.log("before route enter");

    // called before the route that renders this component is confirmed.
    // does NOT have access to `this` component instance,
    // because it has not been created yet when this guard is called!
},

beforeRouteLeave(){
	console.log("before route leave");
},*/


computed : { 
	normalUser(){
		return this.userRole === 3 || this.userRole === 4 || this.userRole === 5 ; 
	},

	full_name(){
		return this.criminals.full_name === null ? "Lorem Ipsum" : "Nothing so far" ;
	},

	fetchCriminalsInfoEndpoint(){
		return app +'/api/v1/criminals/'+this.criminals.id;
	},

	userRole(){
		return user.role_id ;
	},

	avatarPath(criminal){
		return '/assets/images/'+this.criminals.photo ;
	},

	criminalBounty(){
		let bounty = this.criminals.profile.bounty ;
		return bounty ;
		 // === null ? 'No Profile was listed' : this.criminals.profile.bounty + " " +this.criminals.profile.currency ;
		},


	criminalsDetails() {
	// _.head(this.criminalDetails) ;

	_.sortBy(this.criminals, value => {
	// console.log(value);
	return value ; 
	});

	},

criminalInfo(criminalsInfo){
	_.sortBy(this.criminals, value => {
		// console.log(value);
		return value ; 

	});

}

},

created(){
	this.check_if_the_currently_logged_on_user_is_the_creator(); 
}
};
</script>

<style lang="scss" scoped>
#trix-toolbar-1 .trix-button-group:not(:first-child) {
	margin-left: 0vw;
}
</style>

<!-- 
Using slots
<criminal-layout>
<p slot="name" class="font-basic tracking-normal text-2xl mb-4 mt-4 font-normal text-black mr-2">
Criminal Profile of {{  criminal.full_name }} 
</p>
</criminal-layout> -->

