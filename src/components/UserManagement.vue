<template>
    <div class="container">
      <div class="card">
        <div class="card-body">
          <h5 class="card-title">User Management</h5>
          <button type="button" class="btn btn-success" @click="showUsers()">
            Show All Users
          </button>
          <button
            type="button"
            class="btn btn-primary"
            data-bs-toggle="modal" 
            data-bs-target="#addUserDialog"
          >
            Add a User
          </button>
        </div>
      </div>
      <table class="table" id="userTable">
        <thead>
          <tr>
            <th scope="col">Index</th>
            <th scope="col">Name</th>
            <th scope="col">isActive</th>
            <th scope="col">Salary</th>
            <th scope="col">Company</th>
            <th scope="col">Operations</th>
          </tr>
        </thead>
        <tbody id="userTableBody">
          <tr
            v-for="user in table"
            :key="user._id"
          >
            <th>
              <input 
                type="text"
                :value="user.index"
                scope="row"
                class="form-control-plaintext"
              >
            </th>
            <td>
              <input 
                type="text"
                :value="user.name"
                class="form-control-plaintext"
              >
            </td>
            <td>
              <input 
                type="text"
                :value="user.isActive"
                class="form-control-plaintext"
              >
            </td>
            <td>
              <input 
                type="text"
                :value="user.salary"
                class="form-control-plaintext"
              >
            </td>
            <td>
              <input 
                type="text"
                :value="user.company"
                class="form-control-plaintext"
              >
            </td>
            <td>
              <button type="button" class="btn btn-info"  @click="showEditDialog(user._id)">Edit</button>
              <button type="button" class="btn btn-danger" @click="del(user._id)">Delete</button>
            </td>
          </tr>
        </tbody>
      </table>

      <!-- Edit a user dialog -->
      <div
        ref="editModal"
        class="modal fade"
        id="editUserDialog"
        tabindex="-1"
        aria-labelledby="exampleModalLabel"
        aria-hidden="true"
      >
        <div class="modal-dialog">
          <div class="modal-content">
            <div class="modal-header">
              <h5 class="modal-title" id="exampleModalLabel">Edit a User</h5>
              <button
                type="button"
                class="btn-close"
                data-bs-dismiss="modal"
                aria-label="Close"
              ></button>
            </div>
            <div class="modal-body">
              <form>
                <div class="form-group row">
                  <label for="username-edit" class="col-sm-2 col-form-label"
                    >Name</label
                  >
                  <div class="col-sm-10">
                    <input
                      type="text"
                      readonly
                      class="form-control-plaintext"
                      id="username-edit"
                      ref="editName"                      
                    />
                  </div>
                </div>
                <div class="form-group row">
                  <label for="salary-edit" class="col-sm-2 col-form-label"
                    >Salary</label
                  >
                  <div class="col-sm-10">
                    <input
                      type="text"
                      class="form-control"
                      id="salary-edit"
                      ref="editSalary"
                      v-model="editForm.salary"
                      
                    />
                  </div>
                </div>
              </form>
            </div>
            <div class="modal-footer">
              <button
                type="button"
                class="btn btn-secondary"
                data-bs-dismiss="modal"
              >
                Close
              </button>
              <button
                type="button"
                class="btn btn-primary"
                data-bs-dismiss="modal"
                @click="updateUser()"
              >
                Save changes
              </button>
            </div>
          </div>
        </div>
      </div>
      <!-- Add a user dialog -->
      <div
        class="modal fade"
        id="addUserDialog"
        tabindex="-1"
        aria-labelledby="exampleModalLabel"
        aria-hidden="true"
      >
        <div class="modal-dialog">
          <div class="modal-content">
            <div class="modal-header">
              <h5 class="modal-title" id="exampleModalLabel">Add a User</h5>
              <button
                type="button"
                class="btn-close"
                data-bs-dismiss="modal"
                aria-label="Close"
              ></button>
            </div>
            <div class="modal-body">
              <form>
                <div class="form-group row">
                  <label for="username-add" class="col-sm-2 col-form-label"
                    >Name</label
                  >
                  <div class="col-sm-10">
                    <input
                      type="text"
                      class="form-control"
                      id="username-add"
                      v-model="newUser.name"
                      
                    />
                  </div>
                </div>
                <div class="form-group row">
                  <label for="company-add" class="col-sm-2 col-form-label"
                    >Company</label
                  >
                  <div class="col-sm-10">
                    <input
                      type="text"
                      class="form-control"
                      id="company-add"
                      v-model="newUser.company"
                    />
                  </div>
                </div>
                <div class="form-group row">
                  <label for="salary-add" class="col-sm-2 col-form-label"
                    >Salary</label
                  >
                  <div class="col-sm-10">
                    <input
                      type="text"
                      class="form-control"
                      id="salary-add"
                      v-model="newUser.salary"
                    />
                  </div>
                </div>
              </form>
            </div>
            <div class="modal-footer">
              <button
                type="button"
                class="btn btn-secondary"
                data-bs-dismiss="modal"
              >
                Close
              </button>
              <button type="button" class="btn btn-primary" data-bs-dismiss="modal" @click="addUser()">
                Add
              </button>
            </div>
          </div>
        </div>
      </div>
    </div>
</template>

<script>
    export default {
    created() {
       this.userList = this.userList;
       this.table = this.table;
    },
    data() {
        return {
        table: [],
        userList: [
          {
            _id: '5e5d8a33962020b0583a5ade',
            index: 1,
            isActive: false,
            salary: '$1,878.32',
            picture: 'http://placehold.it/32x32',
            age: 34,
            eyeColor: 'brown',
            name: 'Jewel Sosa',
            gender: 'female',
            company: 'SIGNIDYNE',
            email: 'jewelsosa@signidyne.com',
            phone: '+1 (817) 538-2348',
            address: '291 Kingston Avenue, Bannock, Washington, 8709',
          },
          {
            _id: '5e5d8a33d5668ba005759ac4',
            index: 2,
            isActive: false,
            salary: '$3,559.88',
            picture: 'http://placehold.it/32x32',
            age: 23,
            eyeColor: 'brown',
            name: 'Grimes Olsen',
            gender: 'male',
            company: 'YURTURE',
            email: 'grimesolsen@yurture.com',
            phone: '+1 (926) 502-3084',
            address: '377 Danforth Street, Cleary, Virginia, 6123',
          },
          {
            _id: '5e5d8a333296d622e5170499',
            index: 3,
            isActive: true,
            salary: '$2,300.27',
            picture: 'http://placehold.it/32x32',
            age: 33,
            eyeColor: 'brown',
            name: 'Velasquez Bennett',
            gender: 'male',
            company: 'FUTURITY',
            email: 'velasquezbennett@futurity.com',
            phone: '+1 (820) 452-3033',
            address: '605 Metrotech Courtr, Retsof, Kentucky, 2629',
          },
          {
            _id: '5e5d8a33668e01faecf43a21',
            index: 4,
            isActive: false,
            salary: '$2,123.47',
            picture: 'http://placehold.it/32x32',
            age: 38,
            eyeColor: 'green',
            name: 'Gayle Burt',
            gender: 'female',
            company: 'VURBO',
            email: 'gayleburt@vurbo.com',
            phone: '+1 (847) 419-3818',
            address: '319 Highland Avenue, Elizaville, Arkansas, 273',
          },
          {
            _id: '5e5d8a33e0911b7fc2d6e9f3',
            index: 5,
            isActive: true,
            salary: '$2,358.41',
            picture: 'http://placehold.it/32x32',
            age: 32,
            eyeColor: 'blue',
            name: 'Lori Price',
            gender: 'female',
            company: 'GRUPOLI',
            email: 'loriprice@grupoli.com',
            phone: '+1 (915) 511-3754',
            address: '904 Canal Avenue, Canoochee, Pennsylvania, 8762',
          },
          {
            _id: '5e5d8a3314b603b9ef876879',
            index: 6,
            isActive: false,
            salary: '$3,752.40',
            picture: 'http://placehold.it/32x32',
            age: 31,
            eyeColor: 'green',
            name: 'Marissa Hines',
            gender: 'female',
            company: 'SNORUS',
            email: 'marissahines@snorus.com',
            phone: '+1 (870) 512-3865',
            address: '326 Montieth Street, Stouchsburg, Guam, 5638',
          },
          {
            _id: '5e5d8a3349fecb478c4037bc',
            index: 7,
            isActive: false,
            salary: '$1,792.48',
            picture: 'http://placehold.it/32x32',
            age: 20,
            eyeColor: 'brown',
            name: 'Polly Payne',
            gender: 'female',
            company: 'STOCKPOST',
            email: 'pollypayne@stockpost.com',
            phone: '+1 (997) 501-2848',
            address: '286 Canda Avenue, Yardville, Indiana, 5281',
          },
        ],
        newUser: {
            _id: '',
          index: 0,
          isActive: true,
          salary: '',
          picture: '',
          age: 0,
          eyeColor: '',
          name: '',
          gender: '',
          company: '',
          email: '',
          phone: '',
          address: ''
        },
          editForm: {
          _id: '',
          name: '',
          salary: '',
        }
        };
    },
    methods: {
        updateUser() {
          console.log("test")
          let userId = this.editForm._id
          let user = this.table.filter(function (users) {
            return users._id == userId;
          })
          user[0].salary = this.editForm.salary;
        },
        showUsers() {
          this.userList = this.userList.concat(this.table);  
          let uniqueUsers = [];
          this.userList.forEach((user) => {
              if (!uniqueUsers.includes(user)) {
                  uniqueUsers.push(user);
              }
          });
          this.table = uniqueUsers;
        },
        showEditDialog(userId) {
          var myModal = new bootstrap.Modal(this.$refs.editModal, {
            keyboard: false
          })
          myModal.toggle()
          let user = this.table.filter(function (users) {
            return users._id == userId;
          })
          this.$refs.editName.value = user[0].name;
          this.$refs.editSalary.value = user[0].salary;
          this.editForm._id = userId;
          this.editForm.name = user[0].name;
          this.editForm.salary = user[0].salary
        },
        del(id) {
          let filter = this.table.filter(function (users) {
            return users._id != id;
          })
          let filter1 = this.userList.filter(function (users) {
            return users._id != id;
          })
          this.userList = filter1;
          this.table = filter;
        },
        addUser() {
            this.newUser._id = this.generateUserId();
            this.newUser.index = this.generateUserIndex();        
            this.table.push(this.newUser);
            this.clearNewUser();
        },
        clearNewUser() {
          this.newUser = {
              _id: '',
              index: 0,
              isActive: true,
              salary: '',
              picture: '',
              age: 0,
              eyeColor: '',
              name: '',
              gender: '',
              company: '',
              email: '',
              phone: '',
              address: ''
            }
        },
        generateUserId() {
          let timestamp = ((new Date().getTime() / 1000) | 0).toString(16);
          return (
            timestamp +
            'xxxxxxxxxxxxxxxx'
              .replace(/[x]/g, function () {
                return ((Math.random() * 16) | 0).toString(16);
              })
              .toLowerCase()
          );
        },
        generateUserIndex() {
          let index = 0;
          for (let i = 0; i < this.userList.length; i++) {
            if (this.userList[i].index > index) {
              index = this.userList[i].index
            }
          }
          for (let i = 0; i < this.table.length; i++) {
            if (this.table[i].index > index) {
              index = this.table[i].index
            }
          }
          return index + 1;
        },

        

    },
    };
</script>

