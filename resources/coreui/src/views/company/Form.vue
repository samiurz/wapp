<template>
    <div class="animated fadeIn">
        <div class="row">
            <div class="col-sm-12">
                <form enctype="multipart/form-data" autocomplete='off' @submit.prevent="submitForm">
                    <div class="card border-0">
                        <div class="form-group form-actions">
                            <div class="form-group form-actions">
                                <router-link :to="{ name: 'Company List'}">
                                    <button type="button" class="btn btn-sm btn-warning pull-right">Cancel</button>
                                </router-link>
                                <button type="button" @click.prevent="submit" style="margin-right:10px" class="btn btn-sm btn-primary pull-right">Submit</button>
                            </div>
                        </div>
                        <div class="card-body">
                            <div class="row">
                                <div class="form-group col-sm-4">
                                    <b-form-fieldset label="Company Name *">
                                        <b-form-input type="text" :class="errors.has('name') ? 'form-control is-invalid':'form-control'" v-validate="'required'"
                                            name="name" v-model="company.name" placeholder="Please enter company name"></b-form-input>
                                    </b-form-fieldset>
                                </div>
                                <div class="form-group col-sm-4">
                                    <b-form-fieldset label="Website">
                                        <b-form-input type="text" v-model="company.website" placeholder="ex.www.website.com"></b-form-input>
                                    </b-form-fieldset>
                                </div>
                                <div class="form-group col-sm-4">
                                    <label for="">Company Type *</label>
                                    <!-- <select class="form-control" :class="errors.has('type') ? 'form-control is-invalid':'form-control'" v-validate="'required'"
                                        name="type" v-model="company.type">
                                        <option value="0" selected>Select Type</option>
                                        <option value="Distributor">Distributor</option>
                                        <option value="Customer">Customer</option>
                                        <option value="Supplier">Supplier</option>
                                        <option value="Manufacturer">Manufacturer</option>
                                    </select> -->
                                    <b-form-select v-model="companyType" class="mb-3">
                                        <option :value="null">Please select an option</option>
                                        <option value="a">Option A</option>
                                        <option value="b" disabled>Option B (disabled)</option>
                                        <optgroup label="Grouped Options">
                                            <option :value="{'C':'3PO'}">Option with object value</option>
                                            <option :value="{'R':'2D2'}">Another option with object value</option>
                                        </optgroup>
                                    </b-form-select>
                                    <div>Selected:
                                        <strong>{{ selected }}</strong>
                                    </div>
                                </div>
                            </div>
                            <div class="row">
                                <div class="form-group col-sm-4">
                                    <b-form-fieldset label="Primary Address">
                                        <b-form-input type="text" v-model="company.address" placeholder="Enter Your address"></b-form-input>
                                    </b-form-fieldset>

                                </div>
                                <div class="form-group col-sm-4">
                                    <b-form-fieldset label="post Code">
                                        <b-form-input type="text" v-model="company.post_code" placeholder="Please enter your post code"></b-form-input>
                                    </b-form-fieldset>
                                </div>
                                <div class="form-group col-sm-4">
                                    <b-form-fieldset label="Phone Number">
                                        <b-form-input type="text" v-model="company.phone_number" placeholder="phone number"></b-form-input>
                                    </b-form-fieldset>
                                </div>
                            </div>
                            <div class="row">
                                <div class="form-group col-sm-6">
                                    <label for="">Upload Logo</label>
                                    <picture-input ref="pictureInput" width="200" height="200" margin-left="0" accept="image/jpeg,image/png" size="100" button-class="btn"
                                        :custom-strings="{
                                        upload: '<h1>Bummer!</h1>',
                                        drag: 'Drag a 😺 GIF or GTFO'
                                    }" @change="onChange">
                                    </picture-input>
                                </div>
                            </div>
                            <!-- {{company}} {{edit}} -->
                        </div>
                    </div>
                </form>
            </div>
        </div>
        <div :hidden="!edit" class="col-sm-12">
            <div class="card">
                <div class="card-body">
                    <div class="card-header">
                        <router-link :to="{ path: `/user/${$route.params.id}/create`, params: $route.params.id}">
                            <i class="fa fa-plus fa-lg pull-right" title="Add Contact" style="font-size:20px;  color: black;"></i>
                        </router-link>
                        <br>
                    </div>
                    <div class="table-responsive">
                        <table class="table">
                            <thead>
                                <tr>
                                    <th scope="col">ID</th>
                                    <th scope="col">Name</th>
                                    <th scope="col">User Name</th>
                                    <th scope="col">Phone Number</th>
                                    <th scope="col">Email</th>
                                    <!-- <th scope="col">Created By</th>
                                    <th scope="col">Updated By</th> -->
                                    <th scope="col">Action</th>
                                </tr>
                            </thead>
                            <tbody>
                                <tr v-for="user in users" v-bind:key="user.id">
                                    <td>{{ user.id }}</td>
                                    <td>{{ user.last_name +' , '+ user.first_name }}</td>
                                    <td>{{ user.username }}</td>
                                    <td>{{ user.phone_number }}</td>
                                    <td>{{ user.email }}</td>
                                    <!-- <td>{{ user.created_by }}</td>
                                    <td>{{ user.updated_by }}</td> -->
                                    <td>
                                        <router-link :to="{ path: `/user/${user.id}/edit`}">
                                            <i class="fa fa-edit fa-lg" title="Edit Contact" style="font-size:20px;  color: black;"></i>
                                        </router-link>
                                        <!-- <button @click="deleteContact(contact.user_id)" class="btn btn-danger">Delete</button> -->
                                        <router-link :to="{ path:`/company/${company.id}`}">
                                            <span v-on:click="eraseContact(user.id)">
                                                <i class="fa fa-trash fa-lg" title="Delete Contact" style="font-size:20px;  color: black;"></i>
                                            </span>
                                        </router-link>
                                    </td>
                                </tr>
                            </tbody>
                        </table>
                    </div>
                </div>
            </div>
            <div class="card">
                <div class="card-header">
                    <router-link :to="{ path: `/location/${$route.params.id}/create`, params: $route.params.id}">
                        <i class="fa fa-plus fa-lg pull-right" title="Add Location" style="font-size:20px;  color: black;"></i>
                    </router-link>
                    <br>
                </div>
                <div class="table-responsive">
                    <table class="table">
                        <thead>
                            <tr>
                                <th scope="col">ID</th>
                                <th scope="col">Address</th>
                                <th scope="col">Post Code</th>
                                <th scope="col">Phone Number</th>
                                <!-- <th scope="col">Created By</th>
                                <th scope="col">Updated By</th> -->
                                <th scope="col">Action</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr v-for="location in locations" v-bind:key="location.id">
                                <td>{{ location.id }}</td>
                                <td>{{ location.address }}</td>
                                <td>{{ location.post_code }}</td>
                                <td>{{ location.phone_number }}</td>
                                <!-- <td>{{ location.created_by }}</td>
                                <td>{{ location.updated_by }}</td> -->
                                <td>
                                    <router-link :to="{ path: `/location/${location.id}`}">
                                        <i class="fa fa-edit fa-lg" title="Edit Location" style="font-size:20px;  color: black;"></i>
                                    </router-link>
                                    <!-- <button @click="deleteContact(contact.user_id)" class="btn btn-danger">Delete</button> -->
                                    <router-link :to="{ path: `/company/${company.id}`}">
                                        <span v-on:click="eraseLocation(location.id)">
                                            <i class="fa fa-trash fa-lg" title="Delete Location" style="font-size:20px;  color: black;"></i>
                                        </span>
                                    </router-link>
                                </td>
                            </tr>
                        </tbody>
                    </table>
                </div>
            </div>
        </div>
        <!-- {{users}} -->


    </div>
</template>

<script>
    import PictureInput from 'vue-picture-input'
    export default {
        name: "forms",
        components: {
            PictureInput
        },
        data() {
            return {
                users: [],
                locations: [],
                companyType: null,
                company: {
                    is_enable: 1,
                },
                edit: false
            };
        },
        created() {
            console.log(this.$store.state.user);
            if (!this.$store.state.user) this.$router.push("/login");
            this.company.account_id = this.$store.state.user.account_id;

            // this.fetchContactByCompany()
            if (this.$route.params.id) {
                this.edit = true;
                //for edit by id
                this.fetchById(this.$route.params.id);
                console.log("look for id", this.$route.params.id);
            }
        },
        methods: {
            onChange(image) {
                console.log('New picture selected!')
                if (image) {
                    console.log('Picture loaded.')
                    this.company.image = image
                    console.log("image preview", this.company.image)
                } else {
                    console.log('FileReader API not supported: use the <form>, Luke!')
                }
            },
            submit() {
                this.$validator.validate().then(result => {
                    if (result)
                        this.submitForm()
                })
            },
            fetchContactByCompany() {
                this.$http.get(`/api/users/${this.$route.params.id}/company`,{
						headers: {
							Authorization: `Bearer ${this.$store.state.token}`
						}
					})
                    .then(response => {
                        this.users = response.data.users;
                        console.log("users", this.users)
                    })
            },
            //sweet alert
            showAlert() {
                this.$swal('Company Added Successfully');
            },
            submitForm() {
                if (this.edit) {
                    this.company.updated_by = this.$store.state.user.id;
                    this.company.account_id = this.$store.state.user.account_id;
                    this.$http
                        .put(`/api/company`, this.company)
                        .then(response => {
                            this.$router.push("/companies/");
                        })
                        .catch(error => {
                            this.$setErrorsFromResponse(err.response.data);
                            this.$emit("error", err.response.data);
                        });
                } else {
                    this.company.created_by = this.$store.state.user.id;
                    this.$http
                        .post(`/api/company`, this.company)
                        .then(response => {
                            this.$router.push("/companies/");
                        })
                        .catch(error => {
                            this.$setErrorsFromResponse(err.response.data);
                            this.$emit("error", err.response.data);
                        });
                }
            },
            fetchById(id) {
                this.$http.get(`/api/company/${id}`,
                {
						headers: {
							Authorization: `Bearer ${this.$store.state.token}`
						}
                    })
                    .then(response => {
                    this.company = response.data.data;
                    this.users = response.data.data.user;
                    this.locations = response.data.data.location;
                    console.log("company", this.company);
                    console.log("user", this.users);
                    console.log("location", this.locations);
                });
            },
            eraseLocation(id) {
                if (confirm("Are You Sure?")) {
                    this.$http.delete(`/api/location/${id}`,{
						headers: {
							Authorization: `Bearer ${this.$store.state.token}`
						}
					})
                    .then(response => {
                            alert("Location Removed");
                            this.fetchById(this.$route.params.id);
                        })
                }
            },
            eraseContact(id) {
                if (confirm("Are You Sure?")) {
                    this.$http.delete(`/api/user/${id}`,{
						headers: {
							Authorization: `Bearer ${this.$store.state.token}`
						}
					})
                    .then(response => {
                            alert("Contact Removed");
                            this.fetchById(this.$route.params.id);
                        })
                }
            },
        }
    };
</script>