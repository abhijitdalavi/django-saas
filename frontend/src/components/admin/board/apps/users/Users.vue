<template>
  <div>
    <section class="content-header">
      <div class="list-inline">
        <h1>
          Users
          <a
            @click="getUser()"
            class="btn btn-primary btn-raised"
            data-toggle="modal"
            data-target="#user-form"
          >
            Create user
          </a>
        </h1>
      </div>
      <ol class="breadcrumb">
        <li>
          <router-link :to="{ name: 'home' }">Home</router-link>
        </li>
        <li class="active">
          Users
        </li>
      </ol>
    </section>
    <section class="content">
      <div class="row">
        <div class="col-sm-12">
          <div class="box">
            <div class="box-body">
              <div v-if="isLoading" class="text-center">
                <spinner-component />
              </div>
              <table v-else id="table" class="table table-bordered table-striped">
                <thead>
                  <tr>
                    <th>Name</th>
                    <th>Username</th>
                    <th>Rol</th>
                    <th>Email</th>
                    <th>Status</th>
                    <th>Actions</th>
                  </tr>
                </thead>
                <tbody>
                  <tr v-for="(user, index) in users" :key="index">
                    <td>{{ user.first_name }} {{ user.last_name }}</td>
                    <td>{{ user.username }}</td>
                    <td>{{ user.rol_name }}</td>
                    <td>{{ user.email }}</td>
                    <td class="text-center">
                      <p
                        :class="`${ user.is_active ? 'bg-green' : 'bg-red' }`"
                        class="badge p-bg"
                      >
                        {{ user.is_active ? 'Active' : 'Inactive' }}
                      </p>
                    </td>
                    <td class="text-center">
                      <a
                        @click="getUser(user.id)"
                        class="btn.btn-app btn-primary btn-sm action-btn"
                        data-toggle="modal"
                        data-target="#user-form"
                      >
                        <i class="fa fa-edit"></i>
                      </a>
                      <a
                        @click="getUser(user.id)"
                        class="btn.btn-app btn-info btn-sm action-btn"
                        data-toggle="modal"
                        data-target="#user-detail"
                      >
                        <i class="fa fa-info-circle"></i>
                      </a>
                      <a
                        @click="getUser(user.id)"
                        :class="`${ user.is_active ? 'btn-danger' : 'btn-success' }`"
                        class="btn.btn-app btn-sm action-btn"
                        data-toggle="modal"
                        data-target="#user-status"
                      >
                        <i :class="`${ user.is_active ? 'fa fa-user-times' : 'fa fa-user-plus' }`"></i>
                      </a>
                    </td>
                  </tr>
                </tbody>
              </table>
            </div>
          </div>
        </div>
      </div>
    </section>
    <user-detail />
    <user-form />
    <user-status />
  </div>
</template>

<script>
import { mapState, mapGetters, mapActions } from 'vuex'
import template from '@/utilities/template'
import UserDetail from './modals/UserDetail'
import UserForm from './modals/UserForm'
import UserStatus from './modals/UserStatus'

export default {
  name: 'users',
  components: {
    UserDetail,
    UserForm,
    UserStatus
  },
  computed: {
    ...mapState('users', [
      'isLoading'
    ]),
    ...mapGetters('users', [
      'users'
    ])
  },
  methods: {
    ...mapActions('users', [
      'getUsers',
      'getUser'
    ])
  },
  created () {
    this.getUsers()
  },
  updated () {
    this.$nextTick(() => { template.reload() })
  }
}
</script>
