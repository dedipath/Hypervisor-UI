<template>
	<div class="content animated fadeIn">
		<h2><i class="fa fa-list" aria-hidden="true"></i> Add Reseller Plan</h2>
		<div class="col-md-6">
			<div class="box storage user">
				<form @submit.prevent="create">
					<h4>General Settings</h4>
					<div class="row">
						<div class="col-md-4">
							<div class="form-group" :class="{'has-error': errors.name}">
								<label> Plan Name</label>
								<input type="text" name="name" class="form-control">
								<span class="help-block" v-if="errors.name">{{ errors.name[0] }}</span>
							</div>
						</div>
						<div class="col-md-4">
							<div class="form-group" :class="{'has-error': errors.user_count}">
								<label> User Count</label>
								<input type="number" name="user_count" class="form-control">
								<span class="help-block" v-if="errors.user_count">{{ errors.user_count[0] }}</span>
							</div>
						</div>
						<div class="col-md-4">
							<div class="form-group" :class="{'has-error': errors.instance_count}">
								<label> Instance Count</label>
								<input type="number" name="instance_count" class="form-control">
								<span class="help-block" v-if="errors.instance_count">{{
										errors.instance_count[0]
									}}</span>
							</div>
						</div>
					</div>
					<br>
					<h4>CPU & RAM Settings</h4>
					<div class="row">
						<div class="col-md-6">
							<div class="form-group" :class="{'has-error': errors.ram}">
								<label> RAM</label>
								<div class="input-group">
									<input type="number" name="ram" class="form-control">
									<div class="input-group-addon">
										MB
									</div>
								</div>
								<span class="help-block" v-if="errors.ram">{{ errors.ram[0] }}</span>
							</div>
						</div>
						<div class="col-md-6">
							<div class="form-group" :class="{'has-error': errors.cpu_cores}">
								<label> CPU Cores</label>
								<input type="number" name="cpu_cores" class="form-control">
								<span class="help-block" v-if="errors.cpu_cores">{{ errors.cpu_cores[0] }}</span>
							</div>
						</div>
					</div>
					<div class="row">
						<div class="col-md-6">
							<div class="form-group">
								<label> CPU Units</label>
								<input type="number" name="cpu_units" class="form-control">
							</div>
						</div>
						<div class="col-md-6">
							<div class="form-group">
								<label> CPU Percent</label>
								<input type="number" name="cpu_percent" class="form-control">
							</div>
						</div>
					</div>
					<br>
					<h4>Storage Settings</h4>
					<div class="row">
						<div class="col-md-6">
							<div class="form-group" :class="{'has-error': errors.disk_size}">
								<label> Disk Storage</label>
								<div class="input-group">
									<input type="number" name="disk_size" class="form-control">
									<div class="input-group-addon">
										GB
									</div>
								</div>
								<span class="help-block" v-if="errors.disk_size">{{ errors.disk_size[0] }}</span>
							</div>
						</div>
						<div class="col-md-6">
							<div class="form-group">
								<label>Provision Disk</label>
								<select name="disk_type" class="form-control">
									<option value="any">Any</option>
									<option value="hdd">HDD</option>
									<option value="sas">SAS</option>
									<option value="ssd_cached">SSD Cached</option>
									<option value="ssd">SSD</option>
									<option value="nvme">NVMe</option>
								</select>
							</div>
						</div>
					</div>
					<div class="row">
						<div
							:class="{'col-md-4': disk_driver === 'virtio-scsi', 'col-md-6': disk_driver !== 'virtio-scsi'}">
							<div class="form-group">
								<label> Disk Driver</label>
								<select name="disk_driver" class="form-control" data-width="100%">
									<option value="ide">IDE</option>
									<option value="virtio">Virtio</option>
									<option value="virtio-scsi">Virtio SCSI</option>
									<option value="scsi">SCSI</option>
								</select>
							</div>
						</div>
						<div
							:class="{'col-md-4': disk_driver === 'virtio-scsi', 'col-md-6': disk_driver !== 'virtio-scsi'}">
							<div class="form-group">
								<label> Disk Cache</label>
								<select name="disk_cache" class="form-control" data-width="100%">
									<option value="none">Default</option>
									<option value="writethrough">Write Through</option>
									<option value="writeback">Write Back</option>
									<option value="directsync">Direct Sync</option>
								</select>
							</div>
						</div>
						<div class="col-md-4" v-show="disk_driver === 'virtio-scsi'">
							<div class="form-group">
								<label> Disk Discard</label>
								<select name="disk_discard" class="form-control" data-width="100%">
									<option value="none">None</option>
									<option value="unmap">Unmap</option>
								</select>
							</div>
						</div>
					</div>
					<br>
					<h4>I/O Settings</h4>
					<div class="row">
						<div class="col-md-6">
							<div class="form-group">
								<label>I/O Mode</label>
								<select name="io_mode" class="form-control">
									<option value="native">Native</option>
									<option value="native">Threads</option>
								</select>
							</div>
						</div>
						<div class="col-md-6">
							<div class="form-group">
								<label>Total IOPS</label>
								<input type="number" name="io_total" class="form-control" value="0">
							</div>
						</div>
					</div>
					<div class="row">
						<div class="col-md-6">
							<div class="form-group">
								<label>I/O Reads</label>
								<div class="input-group">
									<input type="number" name="io_reads" class="form-control" value="0">
									<div class="input-group-addon">
										MB/s
									</div>
								</div>
							</div>
						</div>
						<div class="col-md-6">
							<div class="form-group">
								<label>I/O Writes</label>
								<div class="input-group">
									<input type="number" name="io_writes" class="form-control" value="0">
									<div class="input-group-addon">
										MB/s
									</div>
								</div>
							</div>
						</div>
					</div>
					<br>
					<h4>Network Settings</h4>
					<div class="row">
						<div class="col-md-6">
							<div class="form-group">
								<label> Bandwidth</label>
								<div class="input-group">
									<input type="number" name="bandwidth" class="form-control">
									<div class="input-group-addon">
										GB
									</div>
								</div>
							</div>
						</div>
						<div class="col-md-6">
							<div class="form-group" :class="{'has-error': errors.bandwidth_accounting}">
								<label>Bandwidth Accounting</label>
								<select name="bandwidth_accounting" class="form-control" data-width="100%">
									<option value=""></option>
									<option value="uploads">Uploads</option>
									<option value="downloads">Downloads</option>
									<option value="both">Both</option>
								</select>
								<span class="help-block"
									  v-if="errors.bandwidth_accounting">{{ errors.bandwidth_accounting[0] }}</span>
							</div>
						</div>
					</div>
					<div class="row">
						<div class="col-md-6">
							<div class="form-group" :class="{'has-error': errors.bandwidth_overage}">
								<label>Overage Action</label>
								<select name="bandwidth_overage" class="form-control" data-width="100%">
									<option value=""></option>
									<option value="suspend_network">Suspend Network</option>
									<option value="suspend_instance">Suspend Instance</option>
									<option value="billing">Bill Overage</option>
								</select>
								<span class="help-block" v-if="errors.bandwidth_overage">{{
										errors.bandwidth_overage[0]
									}}</span>
							</div>
						</div>
						<div class="col-md-6" v-if="bandwidth_overage === 'billing'">
							<div class="form-group" :class="{'has-error': errors.overage_price}">
								<label>Bandwidth Overage Price</label>
								<div class="input-group">
									<div class="input-group-addon">{{
											currencySymbol[environment.settings.currency_code]
										}}
									</div>
									<input type="number" name="overage_price" class="form-control"
										   :disabled="!enabledBilling">
									<div class="input-group-addon">/GB</div>
								</div>
								<span class="help-block" v-if="errors.overage_price">{{
										errors.overage_price[0]
									}}</span>
							</div>
						</div>
						<div class="col-md-6" v-else>
							<div class="form-group">
								<label>IPv4 Count</label>
								<input type="number" name="ipv4_count" class="form-control" value="0">
							</div>
						</div>
					</div>
					<div class="row">
						<div v-if="bandwidth_overage === 'billing'" class="col-md-4">
							<div class="form-group">
								<label>IPv4 Count</label>
								<input type="number" name="ipv4_count" class="form-control" value="0">
							</div>
						</div>
						<div
							:class="{'col-md-6': bandwidth_overage !== 'billing', 'col-md-4': bandwidth_overage === 'billing'}">
							<div class="form-group">
								<label>IPv6 Count</label>
								<input type="number" name="ipv6_count" class="form-control" value="0">
							</div>
						</div>
						<div
							:class="{'col-md-6': bandwidth_overage !== 'billing', 'col-md-4': bandwidth_overage === 'billing'}">
							<div class="form-group">
								<label>IPv6 Subnet Count</label>
								<input type="number" name="ipv6_subnet_count" class="form-control" value="0">
							</div>
						</div>
					</div>
					<div class="row">
						<div class="col-md-6">
							<div class="form-group">
								<label>Upload Speed</label>
								<div class="input-group">
									<input type="number" name="upload_speed" class="form-control" value="0">
									<div class="input-group-addon">
										Mbit/s
									</div>
								</div>
							</div>
						</div>
						<div class="col-md-6">
							<div class="form-group">
								<label>Download Speed</label>
								<div class="input-group">
									<input type="number" name="download_speed" class="form-control" value="0">
									<div class="input-group-addon">
										Mbit/s
									</div>
								</div>
							</div>
						</div>
					</div>
					<br>
					<h4>Other Settings</h4>
					<div class="row">
						<div :class="{'col-md-4': enabledBilling, 'col-md-6': !enabledBilling}">
							<div class="form-group">
								<label>Regions</label>
								<select name="regions[]" class="form-control" multiple="multiple" data-width="100%">
									<option value=""></option>
								</select>
							</div>
						</div>
						<div :class="{'col-md-4': enabledBilling, 'col-md-6': !enabledBilling}">
							<div class="form-group">
								<label>Image Groups</label>
								<select name="image_groups[]" class="form-control" multiple="multiple"
										data-width="100%">
									<option value=""></option>
								</select>
							</div>
						</div>
						<div class="col-md-4" v-if="enabledBilling">
							<div class="form-group" :class="{'has-error': errors.price}">
								<label>Price</label>
								<div class="input-group">
									<div class="input-group-addon">
										{{ currencySymbol[environment.settings.currency_code] }}
									</div>
									<input type="text" name="price" class="form-control">
									<div class="input-group-addon">
										/mo
									</div>
								</div>
								<span class="help-block" v-if="errors.price">{{ errors.price[0] }}</span>
							</div>
						</div>
					</div>
					<br/>
					<button class="btn btn-primary" type="submit" :disabled="processing">
						<i v-if="!processing" class="fa fa-plus" aria-hidden="true"></i>
						<i v-else class="fa fa-spin fa-spinner"></i>
						Create Plan
					</button>
				</form>
			</div>
		</div>
	</div>
</template>
<script>
export default {
	layout: 'admin',
	head: {
		title: 'Create Reseller Plan'
	},
	data() {
		return {
			bandwidth_overage: '',
			processing: false,
			disk_driver: 'ide',
			duplicate: {}
		}
	},
	async asyncData({$axios, route}) {
		if (route.query.duplicate) {
			if (route.query.duplicate !== '') {
				let response = await $axios.get('/admin/reseller-plans/' + route.query.duplicate);
				return {
					duplicate: response.data,
					disk_driver: response.data.disk_driver,
					bandwidth_overage: response.data.bandwidth_overage
				}
			}
		}
	},
	methods: {
		create() {
			this.processing = true;
			this.$axios.post('/admin/reseller-plans', $('form').serialize()).then((response) => {
				this.processing = false;
				this.$router.push({name: 'admin-reseller-plan-id', params: {id: response.data.reseller_plan.id}});
			}).catch((error) => {
				this.processing = false;
			});
		}
	},
	mounted() {
		let vm = this, regions = [], image_groups = [];
		$('[name="bandwidth_overage"]').select2({placeholder: 'Overage Action'}).on('change', function () {
			vm.bandwidth_overage = this.value;
		});
		$('[name="bandwidth_accounting"]').select2({placeholder: 'Select Accounting'});
		$('[name="disk_driver"]').select2({placeholder: 'Select Driver'}).on('change', function () {
			vm.disk_driver = this.value;
		});
		$('[name="disk_cache"]').select2({placeholder: 'Select Cache'});
		$('[name="disk_discard"]').select2({placeholder: 'Select Discard'});
		$('[name="disk_type"]').select2({placeholder: 'Select Discard'});
		if (this.duplicate.id) {
			for (let key in this.duplicate) {
				if (this.duplicate.hasOwnProperty(key)) {
					if (['regions', 'image_groups'].indexOf(key) === -1) {
						$('[name="' + key + '"]').val(this.duplicate[key]).trigger('change');
					}
					if (key === 'regions') {
						this.duplicate[key].forEach((region) => {
							regions.push({id: region.id, text: region.name, selected: true});
						});
					} else if (key === 'image_groups') {
						this.duplicate[key].forEach((image_group) => {
							image_groups.push({id: image_group.id, text: image_group.name, selected: true});
						});
					}
				}
			}
		}
		$('[name="regions[]"]').select2({
			placeholder: 'Select Region',
			ajax: {
				url: this.$axios.defaults.baseURL + '/admin/regions',
				headers: {
					"Authorization": window.localStorage.getItem('auth._token.local'),
					"X-Requested-With": 'XMLHttpRequest',
					"Content-Type": "application/json",
				},
				dataType: 'json',
				delay: 250,
				data: function (params) {
					return {
						search: $.trim(params.term),
					}
				},
				processResults: function (data) {
					let regions = [];
					data.data.forEach((region) => {
						regions.push({id: region.id, text: region.name});
					});
					return {
						results: regions
					}
				},
				cache: true
			},
			data: regions
		});
		$('[name="image_groups[]"]').select2({
			placeholder: 'Select Groups',
			ajax: {
				url: this.$axios.defaults.baseURL + '/admin/image-groups',
				headers: {
					"Authorization": window.localStorage.getItem('auth._token.local'),
					"X-Requested-With": 'XMLHttpRequest',
					"Content-Type": "application/json",
				},
				dataType: 'json',
				delay: 250,
				data: function (params) {
					return {
						search: $.trim(params.term),
					}
				},
				processResults: function (data) {
					let groups = [];
					data.data.forEach((group) => {
						groups.push({id: group.id, text: group.name});
					});
					return {
						results: groups
					}
				},
				cache: true
			},
			data: image_groups
		});
	}
}
</script>
