<template>
  <div class="form-box">
    <registration-form @create="createUser"/>
    <registration-table :userinfo="this.userinfo"
                        @duplicate="duplicateSelectedRows" @delete="deleteSelectedRows"/>
  </div>
</template>

<script>
import RegistrationForm from "@/components/RegistrationForm.vue";
import RegistrationTable from "@/components/RegistrationTable.vue";

export default {
  components: {
    RegistrationForm, RegistrationTable
  },
  data() {
    return {
      userinfo: [
        {
          id: Date.now(), email: 'example@gmail.com', password: 'qwerty123', surname: 'Петренко', firstname: 'Петро',
          middleName: 'Петрович', gender: "Чоловік", phone: "+38(012) 345-67-89", birthDate: "1983-10-11",
          group: "IA-22", selected: false
        }
      ],
    }
  },
  methods: {
    createUser(form) {
      this.userinfo.push(form);
    },
    deleteSelectedRows() {
      this.userinfo = this.userinfo.filter(user => !user.selected);
    },
    duplicateSelectedRows() {
      const selectedUsers = this.userinfo.filter(user => user.selected);
      const duplicatedUsers = selectedUsers.map(user => ({...user, id: Date.now(), selected: false}));
      this.userinfo = [...this.userinfo, ...duplicatedUsers];
    }
  }
};
</script>
<style>
body {
  font-family: Arial, sans-serif;
  background: linear-gradient(to bottom right, white, blue, violet);
  background-size: cover;
  margin: 0;
  padding: 0;
  display: flex;
  justify-content: center;
  align-items: center;
  height: 125vh;
}

.form-box {
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
  border: 2px solid rgba(255, 255, 255, 0.5);
  border-radius: 20px;
}

h2 {
  font-size: 2em;
  color: #fff;
  text-align: center;
}

.input-box label {
  position: absolute;
  left: 5px;
  margin-top: -5px;
  color: #fff;
}

.input-box input, .input-phone input, .input-date input {
  width: 100%;
  height: 50px;
  background: transparent;
  border: none;
  outline: none;
  font-size: 1em;
  padding: 0 35px 0 5px;
  color: white;
}

.input-group select {
  width: 100%;
  height: 50px;
  background: transparent;
  border: none;
  font-size: 1em;
  padding: 0 35px 0 5px;
}

button {
  width: 29%;
  margin-left: 376px;
  height: 40px;
  border-radius: 40px;
  background: #fff;
  border: none;
  outline: none;
  cursor: pointer;
  font-size: 1em;
  font-weight: 600;
}

table {
  width: 60%;
  margin-left: 100px;
  padding: 30px;
  min-height: 90px;
  border-radius: 10px;
  border-color: #fff;
}

th {
  border-radius: 10px;
  border: 2px solid #fff;
  height: 40px;

}

td {
  border-radius: 10px;
  border: 2px solid #fff;
  height: 40px;
}

td button {
  margin-left: 20px;
}

@media (max-width: 768px) {
  body {
    height: auto;
  }

  .form-box {
    padding: 20px;
  }

  button {
    width: 90%;
    margin-left: -0px;
  }

  table {
    width: 100%;
    display: flex;
    margin-left: -0px;
  }

  thead tr {
    display: flex;
    flex-wrap: wrap;
    justify-content: space-between;
  }

  th {
    width: calc(50% - 10px);
    margin-bottom: 10px;
  }

  tbody {
    display: block;
    position: absolute;
    margin-top: 350px;
    margin-left: -50px;
    justify-content: center;
  }

  tbody tr {
    display: flex;
    flex-wrap: wrap;
    justify-content: space-between;
  }

  td {
    width: calc(50% - 10px);
    margin-bottom: 10px;
    overflow: hidden;
  }

  td button {
    margin-left: 0;
    width: 30%;
  }
}
</style>
