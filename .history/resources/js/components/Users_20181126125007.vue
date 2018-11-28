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
                    <th>Modify</th>
                  </tr>
                  <tr>
                    <td>183</td>
                    <td>John Doe</td>
                    <td>11-7-2014</td>
                    <td><span class="tag tag-success">Approved</span></td>
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
          <div class="modal-body">
            <div class="form-group">
              <label>Username</label>
              <input v-model="form.username" type="text" name="username"
                class="form-control" :class="{ 'is-invalid': form.errors.has('username') }">
              <has-error :form="form" field="username"></has-error>
            </div>
          </div>
          <div class="modal-footer">
            <button type="button" class="btn btn-danger" data-dismiss="modal">Close <i class="fas fa-minus-circle"></i></button>
            <button type="button" class="btn btn-primary">Create User <i class="fas fa-plus-circle"></i></button>
          </div>
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
             }) 
          }
        },
        mounted() {
            console.log('Component mounted.')
        }
    }
</script>
