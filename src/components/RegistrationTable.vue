<template>
  <button class="btn" @click="deleteSelectedRows">Видалити</button>
  <button class="btn" @click="duplicateSelectedRows">Дублювати</button>
  <table id="data-table">
    <thead>
    <tr>
      <th>Вибір</th>
      <th>Email</th>
      <th>Пароль</th>
      <th>Прізвище</th>
      <th>Ім'я</th>
      <th>По батькові</th>
      <th>Стать</th>
      <th>Номер телефону</th>
      <th>Дата народження</th>
      <th>Група</th>
    </tr>
    </thead>
    <tbody>
    <tr v-for="user in userinfo" :key="user.id">
      <td><input type="checkbox" v-model="user.selected"></td>
      <td>{{ user.email }}</td>
      <td>{{ user.password }}</td>
      <td>{{ user.surname }}</td>
      <td>{{ user.firstname }}</td>
      <td>{{ user.middleName }}</td>
      <td>{{ user.gender }}</td>
      <td>{{ user.phone }}</td>
      <td>{{ user.birthDate }}</td>
      <td>{{ user.group }}</td>
    </tr>
    </tbody>
  </table>
</template>

<script>
export default {
  props: {
    userinfo: {
      type: Array
    }
  },
  methods: {
    deleteSelectedRows() {
      this.$emit('delete', this.userinfo);
    },
    duplicateSelectedRows() {
      this.$emit('duplicate', this.userinfo);
    }
  }
}
</script>

<style scoped>
.btn {
  width: 10%;
  margin-left: 280px;
  margin-top: 10px;
}

@media (max-width: 768px) {
  .btn {
    width: 30%;
    margin-left: 46px;
  }
}
</style>