<template>
  <div class="profile">
    <div class="container">
      <div class="row py-3 py-lg-5">
        <div class="col-12" v-if="loggedAdmin">
          <form @submit.prevent="updateAdmin">
            <div class="row">
              <div class="col-12 col-lg-6">
                <div class="form-group">
                  <small>Name</small>
                  <input
                    type="text"
                    class="form-control rounded-0 shadow-none"
                    v-model="loggedAdmin.name"
                  />
                </div>
              </div>
              <div class="col-12 col-lg-6">
                <div class="form-group">
                  <small>E-mail</small>
                  <input
                    type="text"
                    class="form-control rounded-0 shadow-none"
                    disabled
                    v-model="loggedAdmin.email"
                  />
                </div>
              </div>
            </div>
            <button type="submit" class="btn btn-block rounded-0 shadow-none">Update</button>
          </form>
        </div>
      </div>

      <div class="row">
        <div class="col-12 col-lg-6 mt-4 mt-lg-0">
          <h5>Other admins</h5>
          <ol class="pl-4">
            <li v-for="(admin, i) in admins" :key="i">
              <h6 class="text-capitalize">{{admin.name}}</h6>
            </li>
          </ol>
        </div>
        <div class="col-12 col-lg-6 mt-4 mb-4 mt-lg-0">
          <h5>Make new admin</h5>
          <form @submit.prevent="saveAdmin">
            <div class="form-group">
              <small class="error" v-if="errors.name_err">{{errors.name_err}}</small>
              <input
                type="text"
                class="form-control rounded-0 shadow-none"
                placeholder="Name"
                v-model="newAdmin.name"
              />
            </div>
            <div class="form-group">
              <small class="error" v-if="errors.email_err">{{errors.email_err}}</small>
              <input
                type="text"
                class="form-control rounded-0 shadow-none"
                placeholder="E-mail"
                v-model="newAdmin.email"
              />
            </div>
            <div class="form-group">
              <small class="error" v-if="errors.password_err">{{errors.password_err}}</small>
              <input
                type="password"
                class="form-control rounded-0 shadow-none"
                placeholder="Password"
                v-model="newAdmin.password"
              />
            </div>
            <button type="submit" class="btn btn-block rounded-0 shadow-none">Save</button>
          </form>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  name: "admin-profile",
  data() {
    return {
      admins: [],
      loggedAdmin: "",
      adminId: localStorage.getItem("id"),
      newAdmin: {
        name: "",
        email: "",
        password: ""
      },
      errors: {
        name_err: "",
        email_err: "",
        password_err: ""
      },
      header: {
        headers: {
          Authorization: "Bearer " + localStorage.getItem("token")
        }
      }
    };
  },
  mounted() {
    this.$axios
      .get(`${this.$admin_api}all-admin`, this.header)
      .then(res => {
        this.admins = res.data.admins;
      })
      .catch(error => {
        if (error) {
          if (error.response.status == 401) {
            localStorage.clear();
            this.$router.push({ path: "/admin" });
          }
        }
      });
    this.$axios
      .get(`${this.$admin_api}logged-admin/` + this.adminId, this.header)
      .then(res => {
        this.loggedAdmin = res.data.admin;
      })
      .catch(error => {
        if (error) {
          if (error.response.status == 401) {
            localStorage.clear();
            this.$router.push({ path: "/admin" });
          }
        }
      });
  },
  methods: {
    saveAdmin() {
      if (!this.newAdmin.name) {
        this.errors.name_err = "Name is required*";
      } else if (!this.newAdmin.email) {
        this.errors.email_err = "E-mail is required*";
      } else if (this.newAdmin.email.indexOf("@gmail.com") < 0) {
        this.errors.email_err = "Address isn't valid*";
      } else if (!this.newAdmin.password) {
        this.errors.password_err = "Password is required*";
      } else {
        this.errors = false;
        this.$axios
          .post(`${this.$admin_api}create-admin`, this.newAdmin)
          .then(res => {
            if (res.data.message === "error") {
              this.$fire({
                title: "Exist",
                text: "This admin already registered !!",
                type: "warning",
                timer: 3000
              });
            }
            if (res.data.message === "success") {
              this.$axios
                .get(`${this.$admin_api}all-admin`, this.header)
                .then(res => {
                  this.admins = res.data.admins;
                })
                .catch(error => {
                  if (error) {
                    if (error.response.status == 401) {
                      localStorage.clear();
                      this.$router.push({ path: "/admin" });
                    }
                  }
                });
              this.$fire({
                title: "Successfully",
                text: "New admin saved !!",
                type: "success",
                timer: 3000
              });
            }
          });
      }
    },
    updateAdmin() {
      this.$axios
        .patch(`${this.$admin_api}update-admin`, this.loggedAdmin, this.header)
        .then(res => {
          if (res.data.message == "success") {
            this.$axios
              .get(`${this.$admin_api}all-admin`, this.header)
              .then(res => {
                this.admins = res.data.admins;
              })
              .catch(error => {
                if (error) {
                  if (error.response.status == 401) {
                    localStorage.clear();
                    this.$router.push({ path: "/admin" });
                  }
                }
              });
            this.$axios
              .get(
                `${this.$admin_api}logged-admin/` + this.adminId,
                this.header
              )
              .then(res => {
                this.loggedAdmin = res.data.admin;
              })
              .catch(error => {
                if (error) {
                  if (error.response.status == 401) {
                    localStorage.clear();
                    this.$router.push({ path: "/admin" });
                  }
                }
              });
            this.$fire({
              title: "Successfully",
              text: "Update your info !!",
              type: "success",
              timer: 3000
            });
          }
        })
        .catch(error => {
          if (error) {
            if (error.response.status == 401) {
              localStorage.clear();
              this.$router.push({ path: "/admin" });
            }
          }
        });
    }
  }
};
</script>
<style lang="scss" scoped>
.profile {
  .form-control {
    height: 50px;
  }
  .form-control:focus {
    border: 1px solid #f87800;
  }
  .btn {
    height: 50px;
    color: #ffffff;
    background: #f87800;
  }
  .error {
    color: #f87800;
  }
}
</style>