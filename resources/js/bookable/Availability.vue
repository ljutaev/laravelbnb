<template>
	<div>
		<h6 class="text-uppercase text-secondary font-weight-bolder">
			Check Availability
			<transition name="fade">
				<span v-if="noAvailability" class="text-danger">Not Available</span>
				<span v-if="hasAvailability" class="text-success">Available</span>
			</transition>
			
		</h6>

		<div class="form-row">
			<div class="form-group col-md-6">
				<label for="from">From</label>
				<input
				  type="text"
				  name="from"
				  class="form-control form-control-sm"
				  placeholder="Start date"
				  v-model="from"
				  @keyup.enter="check"
				  :class="[{'is-invalid': this.errorFor('from')}]"
				>
				<div class="invalid-feedback" v-for="(error, index) in this.errorFor('from')" :key="'from'+index">{{error}}</div>
			</div>
			<div class="form-group col-md-6">
				<label for="to">To</label>
				<input
				  type="text"
				  name="to"
				  class="form-control form-control-sm"
				  placeholder="End date"
				  v-model="to"
				  @keyup.enter="check"
				  :class="[{'is-invalid': this.errorFor('to')}]"
				>
				<div class="invalid-feedback" v-for="(error, index) in this.errorFor('to')" :key="'from'+index">{{error}}</div>
			</div>
		</div>
		<button class="btn btn-secondary btn-block" @click="check" :disabled="loading">
			<span v-if="!loading">Check!</span>
      <span v-if="loading">
        <i class="fas fa-circle-notch fa-spin"></i> Checking...
      </span></button>
	</div>
</template>

<script>
	import {is422} from './../shared/utils/response'

	export default {
		props: {
			bookableId: [String, Number]
		},
		data() {
			return {
				from: this.$store.state.lastSearch.from || null,
				to: this.$store.state.lastSearch.to || null,
				loading: false,
				status: null,
				errors: null
			}
		},
		methods: {
			async check() {
				this.loading = true
				this.errors = null

				this.$store.dispatch('setLastSearch', {
					from: this.from,
					to: this.to
				})

				try {
					this.status = (await axios.get(`/api/bookables/${this.bookableId}/availability?from=${this.from}&to=${this.to}`)).status

					this.$emit('availability', this.hasAvailability)
				} catch(err) {
					if(is422(err)) {
						this.errors = err.response.data.errors
					}
					this.status = err.response.status
					this.$emit('availability', this.hasAvailability)
				}

				this.loading = false
			},
			errorFor(field) {
				return this.hasErrors && this.errors[field] ? this.errors[field] : null
			}
		},
		computed: {
			hasErrors() {
				return 422 == this.status && this.errors != null
			},
			hasAvailability() {
				return 200 == this.status
			},
			noAvailability() {
				return 404 == this.status
			},
		}
	}
</script>

<style scoped>
label {
  font-size: 0.7rem;
  text-transform: uppercase;
  color: gray;
  font-weight: bolder;
}
.is-invalid {
  border-color: #b22222;
  background-image: none;
}
.invalid-feedback {
  color: #b22222;
}
</style>