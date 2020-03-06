<template>
    <div class="container">
	    <div class="row justify-content-center">
	        <div class="col-md-8">
	            <div class="card">
	                <div class="card-header">
	                    Employee
	                    <a @click="create" class="btn btn-primary btn-sm float-right text-white">Add Employee</a>
	                </div>

	                <div class="card-body">
	                    <b-table
	                    	ref="table"
				            :data="data"
				            :columns="columns"
				            :default-sort-direction="'asc'"
				            :sort-icon="'arrow-up'"
				            :sort-icon-size="'is-small'"
				            default-sort="name"
				            >
				            <template slot-scope="props">
	                			<b-table-column field="id">
	                                {{ props.row.id }}
	                            </b-table-column>

	                            <b-table-column field="name" sortable>
	                                {{ props.row.name }}
	                            </b-table-column>

	                            <b-table-column sortable>
	                                {{ props.row.email }}
	                            </b-table-column>

	                            <b-table-column>
	                                <span class="tag is-success">
									    <!-- {{ new Date(props.row.birthdate).toLocaleDateString() }} -->
									    {{ props.row.birthdate }}
									</span>
	                            </b-table-column>

				                <b-table-column>
									{{ props.row.gender }}
				                </b-table-column>

				                <b-table-column label="Actions">
				                	<button @click="edit(props.row, props.rowIndex)" class="btn btn-primary btn-sm">Edit</button>
				                	<button @click="deleteItem(props.index)" type="submit" class="btn btn-xs btn-danger btn-sm"> Delete</button>
				                </b-table-column>
				            </template>
				        </b-table>
	                </div>
	            </div>
	        </div>
	    </div>
	    <b-modal :active.sync="isComponentModalActive"
            has-modal-card  :can-cancel="false">
		    <modal-form :formData="formData"
		    :is-type="isType"
		    @updateData="updateData"
		    @createData="createData"></modal-form>
		</b-modal>
	</div>
</template>


<script>
	import Vuetable from 'vuetable-2'
	import vmodal from 'vue-js-modal'
	import modalForm from './Form';

    export default {
    	data() {
    		return {
    			data: [{
    				"id": 1,
				    "name": "Bhavin",
				    "email": "bhavin@gmail.com",
				    "birthdate": "1994-01-01",
				    "gender": "Male",
			  	},
			  	{
    				"id": 2,
				    "name": "Kiran",
				    "email": "kiran@gmail.com",
				    "birthdate": "1999-10-01",
				    "gender": "Male",
			  	},
			  	{
    				"id": 3,
				    "name": "Kiara",
				    "email": "kiran@gmail.com",
				    "birthdate": "1999-10-01",
				    "gender": "Female",
			  	}
			  	],
            	columns: [
	                {
	                    field: 'id',
	                    label: 'ID',
	                    numeric: true
	                },
	                {
	                    field: 'name',
	                    label: 'Name',
	                    searchable: true,
	                    sort: true,
	                },
	                {
	                    field: 'email',
	                    label: 'Email',
	                    searchable: true,
	                },
	                {
	                    field: 'birthdate',
	                    label: 'Birthdate'
	                },
	                {
	                    field: 'gender',
	                    label: 'Gender',
	                    searchable: true,
	                }
            	],
    			sortOrder: [
			        {
			          field: "email",
			          direction: "asc"
			        }
			    ],
			    isComponentModalActive: false,
			    showModal: false,
			    isType: 'Create',
			    formData: null
    		}
    	},
        components: {
			Vuetable,
			vmodal,
			modalForm
		},
		methods: {
			create() {
				this.isComponentModalActive = true;
				this.isType = 'Create';
			},
			edit(data, rowId) {
				this.isComponentModalActive = true;
				this.isType = 'Edit';
				this.formData = data;
			},
			deleteItem(rowId) {
				this.data.splice(rowId, 1);
			},
			updateData(formData) {
				const index = this.data.findIndex(value => value.id == formData.id)
				this.data[index].name = formData.name;
				this.data[index].email = formData.email;
				this.data[index].birthdate = formData.birthdate;
				this.data[index].gender = formData.gender;
				this.closeModal()
			},
			createData(formData) {
				const latestIndex = this.data.length;
				this.data[latestIndex] = formData;
				this.closeModal()
			},
			closeModal() {
				this.isComponentModalActive = false;
			}
		}
    }
</script>
