<template>
  <div class="form-box">
    <form @submit.prevent>
      <h2>Реєстрація</h2>
      <div class="input-box">
        <label>Email:</label>
        <input v-model="email" @input="clearError('email')" type="text">
        <div class="error-message" id="email-error">{{ errors.email }}</div>
      </div>
      <div class="input-box">
        <label>Пароль:</label>
        <input v-model="password" @input="clearError('password')" type="password">
        <div class="error-message" id="password-error">{{ errors.password }}</div>
      </div>
      <div class="input-box">
        <label>Прізвище:</label>
        <input v-model="surname" @input="clearError('surname')" type="text">
        <div class="error-message" id="surname-error">{{ errors.surname }}</div>
      </div>
      <div class="input-box">
        <label>Ім'я:</label>
        <input v-model="firstname" @input="clearError('firstname')" type="text">
        <div class="error-message" id="first-name-error">{{ errors.firstname }}</div>
      </div>
      <div class="input-box">
        <label>По батькові:</label>
        <input v-model="middleName" @input="clearError('middleName')" type="text">
        <div class="error-message" id="middle-name-error">{{ errors.middleName }}</div>
      </div>
      <div class="input-gender">
        <label>Стать: </label>
        <label>Чоловік</label>
        <input v-model="gender" @input="clearError('gender')" type="radio" value="Чоловік">
        <label>Жінка</label>
        <input v-model="gender" @input="clearError('gender')" type="radio" value="Жінка">
        <div class="error-message" id="gender-error">{{ errors.gender }}</div>
      </div>
      <div class="input-phone">
        <label>Номер телефону:</label>
        <input v-model="phone" v-imask="'+{38}(\\000) 000-00-00'" type="text" @input="clearError('phone')">
        <div class="error-message" id="phone-error">{{ errors.phone }}</div>
      </div>
      <div class="input-date">
        <label>Дата народження:</label>
        <input v-model="birthDate" @input="clearError('birthDate')" type="date" :min="minDate" :max="maxDate">
        <div class="error-message" id="date-error">{{ errors.birthDate }}</div>
      </div>
      <div class="input-group">
        <label>Група:</label>
        <select v-model="group" @change="clearError('groupErr')">
          <option value="ІА-21">ІА-21</option>
          <option value="ІА-22">ІА-22</option>
          <option value="ІА-23">ІА-23</option>
          <option value="ІА-24">ІА-24</option>
        </select>
        <div class="error-message" id="group-error">{{ errors.groupErr }}</div>
      </div>
      <button @click="createUser">Зареєструватися</button>
    </form>
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
  </div>
</template>

<script>
import {IMaskDirective} from "vue-imask";

export default {
  data() {
    return {
      userinfo: [
        {
          id: 1, email: 'example@gmail.com', password: 'qwerty123', surname: 'Петренко', firstname: 'Петро',
          middleName: 'Петрович', gender: "Чоловік", phone: "+38(012) 345-67-89", birthDate: "1983-10-11",
          group: "IA-22", selected: false
        }
      ],
      email: '',
      password: '',
      surname: '',
      firstname: '',
      middleName: '',
      gender: '',
      phone: '',
      birthDate: '',
      group: '',
      errors: {},
      minDate: '1900-06-19',
      maxDate: ''
    }
  },
  methods: {
    createUser() {
      if (!this.validateEmail() || !this.validatePassword() || !this.validateSurname() ||
          !this.validateFirstName() || !this.validateMiddleName() || !this.validateGender() ||
          !this.validatePhone() || !this.validateBirthDate() || !this.validateGroup()) {
        return;
      }
      const newUser = {
        id: Date.now(),
        email: this.email,
        password: this.password,
        surname: this.surname,
        firstname: this.firstname,
        middleName: this.middleName,
        gender: this.gender,
        phone: this.phone,
        birthDate: this.birthDate,
        group: this.group,
      };
      this.userinfo.push(newUser);
      this.email = '';
      this.password = '';
      this.surname = '';
      this.firstname = '';
      this.middleName = '';
      this.gender = '';
      this.phone = '';
      this.birthDate = '';
      this.group = '';
    },
    clearError(fieldId) {
      delete this.errors[fieldId];
    },
    validateEmail() {
      if (!this.email.trim()) {
        this.errors.email = '(!) Заповніть поле Email';
        return false;
      } else if (!this.RegExpEmail(this.email)) {
        this.errors.email = '(!) Невірний формат Email';
        return false;
      } else {
        return true;
      }
    },
    RegExpEmail(email) {
      const emailPattern = /^[a-zA-Z0-9._-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,4}$/;
      return emailPattern.test(email);
    },
    validatePassword() {
      if (!this.password.trim()) {
        this.errors.password = '(!) Заповніть поле Пароль';
        return false;
      } else if (this.password.trim().length < 8) {
        this.errors.password = '(!) Пароль повинен містити принаймні 8 символів';
        return false;
      } else {
        return true;
      }
    },
    validateSurname() {
      if (!this.surname.trim()) {
        this.errors.surname = '(!) Заповніть поле Прізвище';
        return false;
      } else if (!/^[a-zA-Zа-яА-ЯёЁ\s\-']+$/i.test(this.surname)) {
        this.errors.surname = '(!) Поле Прізвище не повинно містити цифри.';
        return false;
      } else {
        return true;
      }
    },
    validateFirstName() {
      if (!this.firstname.trim()) {
        this.errors.firstname = '(!) Заповніть поле Ім\'я';
        return false;
      } else if (!/^[a-zA-Zа-яА-ЯёЁ\s\-']+$/i.test(this.firstname)) {
        this.errors.firstname = '(!) Поле Ім\'я не повинно містити цифри.';
        return false;
      } else {
        return true;
      }
    },
    validateMiddleName() {
      if (!this.middleName.trim()) {
        this.errors.middleName = '(!) Заповніть поле По-батькові';
        return false;
      } else if (!/^[a-zA-Zа-яА-ЯёЁ\s\-']+$/i.test(this.middleName)) {
        this.errors.middleName = '(!) Поле По-батькові не повинно містити цифри.';
        return false;
      } else {
        return true;
      }
    },
    validateGender() {
      if (this.gender === '') {
        this.errors.gender = "(!) Виберіть стать";
        return false;
      } else {
        return true;
      }
    },
    validatePhone() {
      if (this.phone.length >= 18) {
        return true;
      } else {
        this.errors.phone = '(!) Введіть всі цифри в номер';
        return false;
      }
    },
    validateGroup() {
      if (!this.group) {
        this.errors.groupErr = '(!) Виберіть групу';
        return false;
      } else {
        return true;
      }
    },
    validateBirthDate() {
      const birthDate = new Date(this.birthDate);
      const today = new Date();
      const minBirthDate = new Date(1900, 5, 22);

      if (!this.birthDate) {
        this.errors.birthDate = '(!) Заповніть дату народження';
        return false;
      } else if (birthDate > today) {
        this.errors.birthDate = '(!) Дата народження не може бути у майбутньому.';
        return false;
      } else if (birthDate < minBirthDate) {
        this.errors.birthDate = '(!) Мінімальний рік народження - 1900';
        return false;
      } else {
        return true;
      }
    },
    deleteSelectedRows() {
      this.userinfo = this.userinfo.filter(user => !user.selected);
    },
    duplicateSelectedRows() {
      const selectedUsers = this.userinfo.filter(user => user.selected);
      const duplicatedUsers = selectedUsers.map(user => ({...user, id: Date.now(), selected: false}));
      this.userinfo = [...this.userinfo, ...duplicatedUsers];
    }
  },
  mounted() {
    const today = new Date();
    const yesterday = new Date(today);
    yesterday.setDate(today.getDate() - 1);

    const yyyy = yesterday.getFullYear();
    let mm = (yesterday.getMonth() + 1).toString();
    if (mm.length < 2) {
      mm = `0${mm}`;
    }
    let dd = yesterday.getDate().toString();
    if (dd.length < 2) {
      dd = `0${dd}`;
    }

    this.maxDate = `${yyyy}-${mm}-${dd}`;
  },
  directives: {
    imask: IMaskDirective
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

.input-box, .input-gender, .input-phone, .input-date, .input-group {
  position: relative;
  margin: 30px 0 30px 380px;
  width: 310px;
  border-bottom: 2px solid #fff;
  color: white;
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

.error-message {
  color: rgb(255, 0, 0);
  position: absolute;
  top: 110%;
  left: 5px;
  font-size: 1em;
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

.btn {
  width: 10%;
  margin-left: 280px;
  margin-top: 10px;
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

  .input-box, .input-gender, .input-phone, .input-date, .input-group {
    margin: 20px 0;
  }

  button {
    width: 90%;
    margin-left: -0px;
  }

  .btn {
    width: 30%;
    margin-left: 46px;
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
