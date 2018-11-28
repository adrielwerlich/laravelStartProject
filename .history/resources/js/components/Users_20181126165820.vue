<template>
    <div class="container">
<div class="row mt-3">
          <div class="col-md-12">
            <div class="card">
              <div class="card-header">
                <h3 class="card-title">Users List</h3>

                <div class="card-tools">
                  <button class="btn btn-success" data-toggle="modal" data-target="#modalAddNewUser">
                    Add New <i class="fas fa-user-plus"></i>
                  </button>      
                </div>
              </div>
              <!-- /.card-header -->
              <div class="card-body table-responsive p-0">
                <table class="table table-hover">
                  <tbody><tr>
                    <th>ID</th>
                    <th>Name</th>
                    <th>Email</th>
                    <th>Type</th>
                    <th>Created at</th>
                    <th>Modify</th>
                  </tr>
                  <tr v-for="user in users.data" :key="user.id">
                    
                    <td> {{user.id}} </td>
                    <td> {{user.name}} </td>
                    <td style="word-wrap: break-word; max-width: 55px;"> {{user.email}}  </td>
                    <td> {{user.type | upFirstLetter}}  </td>
                    <td> {{user.created_at | humanReadableDate}}  </td>
                    <td>
                      <a href="#" title="edit"><i class="fa fa-edit green"></i></a>  
                      \
                      <a href="#" title="delete"><i class="fa fa-trash red"></i></a>  
                      
                    </td>
                  </tr>

                </tbody></table>
              </div>
              <!-- /.card-body -->
            </div>
            <!-- /.card -->
          </div>
        </div>


    <!-- Modal -->
    <div class="modal fade" id="modalAddNewUser" tabindex="-1" role="dialog" aria-labelledby="addNewUserLabel" aria-hidden="true">
      <div class="modal-dialog modal-dialog-center" role="document">
        <div class="modal-content">
          <div class="modal-header">
            <h5 class="modal-title" id="addNewUserLabel"> Register New User </h5>
            <button type="button" class="close" data-dismiss="modal" aria-label="Close">
              <span aria-hidden="true">&times;</span>
            </button>
          </div>
          <form @submit.prevent="createUser">
          <div class="modal-body">
            <div class="form-group">
              <label>Username</label>
              <input v-model="form.name" type="text" name="name" placeholder="Enter your name"
                class="form-control" :class="{ 'is-invalid': form.errors.has('name') }">
              <has-error :form="form" field="name"></has-error>
            </div>
            <div class="form-group">
              <label>Email</label>
              <input v-model="form.email" type="text" name="email" placeholder="Enter your email"
                class="form-control" :class="{ 'is-invalid': form.errors.has('email') }">
              <has-error :form="form" field="email"></has-error>
            </div>
            <div class="form-group">
              <label>Bio</label>
              <textarea v-model="form.bio" type="text" name="bio" id="bio" placeholder="Short bio (optional)"
                class="form-control" :class="{ 'is-invalid': form.errors.has('bio') }"></textarea>
              <has-error :form="form" field="bio"></has-error>
            </div>
            <div class="form-group">
              <label>Type</label>
              <select name="type" id="type" v-model="form.type" class="form-control" :class="{'is-invalid':form.errors.has('type')}">
                <option value="">Select user role</option>
                <option value="admin">Admin</option>
                <option value="user">Standard user role</option>
                <option value="author">Author</option>
              </select>
              <has-error :form="form" field="type"></has-error>
            </div>
            <div class="form-group">
              <label>Password</label>
              <input v-model="form.password" type="password" name="password" id="password" 
                class="form-control" :class="{ 'is-invalid': form.errors.has('password') }">
              <has-error :form="form" field="bio"></has-error>
            </div>
          </div>
          <!-- modal body -->
          <div class="modal-footer">
            <button type="button" class="btn btn-danger" data-dismiss="modal">Close <i class="fas fa-minus-circle"></i></button>
            <button type="submit" class="btn btn-primary">Create User <i class="fas fa-plus-circle"></i></button>
          </div>

          </form>

        </div>
      </div>
    </div>
    <!-- Modal -->
  
  </div>

</template>

<script>
    export default {
        data() {
          return {
             form: new Form({
               name: '',
               email: '',
               password: '',
               type: '',
               bio: '',
               photo: ''
             }),
              users: {},
              user: {
                name: '',
                email:'',
                bio: '',
                photo: '',
                password: '',
              },

          }
        },
        created(){
          console.log("users created")
          this.fetchUsers();

          // setInterval( () => this.fetchUsers(), 3000 ) every 3 seconds sends a request to refresh users list. Not good practice for performance   

          VueEventsController.$on('userCreated', () => {
            this.fetchUsers()
          })
        },

        methods:{
          createUser() {
            this.$Progress.start()
            this.form.post('api/user').then(res => {
              
              console.log(res)
              
              VueEventsController.$emit('userCreated')
  
              $('#modalAddNewUser').modal('hide')
              
              toast({
                type: 'success',
                title: 'User created in successfully'
              })
              
              this.$Progress.finish()
              
            }).catch(err => {
              console.error(err)
            })

          },
          fetchUsers(){
            
            axios.get('api/user').then(users_list => {
              // debugger
              this.users = users_list.data
              // console.log(this.users)
            });
            
          }
        },
        // mounted() {
        //     console.log('Component mounted.')
        // }
    }
</script>
