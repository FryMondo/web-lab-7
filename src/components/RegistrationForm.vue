<template>
  <form @submit.prevent>
    <h2>Реєстрація</h2>
    <div class="input-box">
      <label>Email:</label>
      <input v-model="form.email" @input="clearError('email')" type="text">
      <div class="error-message" id="email-error">{{ form.errors.email }}</div>
    </div>
    <div class="input-box">
      <label>Пароль:</label>
      <input v-model="form.password" @input="clearError('password')" type="password">
      <div class="error-message" id="password-error">{{ form.errors.password }}</div>
    </div>
    <div class="input-box">
      <label>Прізвище:</label>
      <input v-model="form.surname" @input="clearError('surname')" type="text">
      <div class="error-message" id="surname-error">{{ form.errors.surname }}</div>
    </div>
    <div class="input-box">
      <label>Ім'я:</label>
      <input v-model="form.firstname" @input="clearError('firstname')" type="text">
      <div class="error-message" id="first-name-error">{{ form.errors.firstname }}</div>
    </div>
    <div class="input-box">
      <label>По батькові:</label>
      <input v-model="form.middleName" @input="clearError('middleName')" type="text">
      <div class="error-message" id="middle-name-error">{{ form.errors.middleName }}</div>
    </div>
    <div class="input-gender">
      <label>Стать: </label>
      <label>Чоловік</label>
      <input v-model="form.gender" @input="clearError('gender')" type="radio" value="Чоловік">
      <label>Жінка</label>
      <input v-model="form.gender" @input="clearError('gender')" type="radio" value="Жінка">
      <div class="error-message" id="gender-error">{{ form.errors.gender }}</div>
    </div>
    <div class="input-phone">
      <label>Номер телефону:</label>
      <input v-model="form.phone" v-imask="'+{38}(\\000) 000-00-00'" type="text" @input="clearError('phone')">
      <div class="error-message" id="phone-error">{{ form.errors.phone }}</div>
    </div>
    <div class="input-date">
      <label>Дата народження:</label>
      <input v-model="form.birthDate" @input="clearError('birthDate')"
             type="date" :min="form.minDate" :max="form.maxDate">
      <div class="error-message" id="date-error">{{ form.errors.birthDate }}</div>
    </div>
    <div class="input-group">
      <label>Група:</label>
      <select v-model="form.group" @change="clearError('groupErr')">
        <option value="ІА-21">ІА-21</option>
        <option value="ІА-22">ІА-22</option>
        <option value="ІА-23">ІА-23</option>
        <option value="ІА-24">ІА-24</option>
      </select>
      <div class="error-message" id="group-error">{{ form.errors.groupErr }}</div>
    </div>
    <button @click="createUser">Зареєструватися</button>
  </form>
</template>

<script>
import {IMaskDirective} from "vue-imask";

export default {
  data() {
    return {
      form: {
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
    }
  },
  methods: {
    clearError(errorID) {
      delete this.form.errors[errorID];
    },
    createUser() {
      if (!this.validateEmail() || !this.validatePassword() || !this.validateSurname() ||
          !this.validateFirstName() || !this.validateMiddleName() || !this.validateGender() ||
          !this.validatePhone() || !this.validateBirthDate() || !this.validateGroup()) {
        return;
      }
      this.form.id = Date.now();
      this.$emit('create', this.form)
      this.form = {
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
    validateEmail() {
      if (!this.form.email.trim()) {
        this.form.errors.email = '(!) Заповніть поле Email';
        return false;
      } else if (!this.RegExpEmail(this.form.email)) {
        this.form.errors.email = '(!) Невірний формат Email';
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
      if (!this.form.password.trim()) {
        this.form.errors.password = '(!) Заповніть поле Пароль';
        return false;
      } else if (this.form.password.trim().length < 8) {
        this.form.errors.password = '(!) Пароль повинен містити принаймні 8 символів';
        return false;
      } else {
        return true;
      }
    },
    validateSurname() {
      if (!this.form.surname.trim()) {
        this.form.errors.surname = '(!) Заповніть поле Прізвище';
        return false;
      } else if (!/^[a-zA-Zа-яА-ЯёЁ\s\-']+$/i.test(this.form.surname)) {
        this.form.errors.surname = '(!) Поле Прізвище не повинно містити цифри.';
        return false;
      } else {
        return true;
      }
    },
    validateFirstName() {
      if (!this.form.firstname.trim()) {
        this.form.errors.firstname = '(!) Заповніть поле Ім\'я';
        return false;
      } else if (!/^[a-zA-Zа-яА-ЯёЁ\s\-']+$/i.test(this.form.firstname)) {
        this.form.errors.firstname = '(!) Поле Ім\'я не повинно містити цифри.';
        return false;
      } else {
        return true;
      }
    },
    validateMiddleName() {
      if (!this.form.middleName.trim()) {
        this.form.errors.middleName = '(!) Заповніть поле По-батькові';
        return false;
      } else if (!/^[a-zA-Zа-яА-ЯёЁ\s\-']+$/i.test(this.form.middleName)) {
        this.form.errors.middleName = '(!) Поле По-батькові не повинно містити цифри.';
        return false;
      } else {
        return true;
      }
    },
    validateGender() {
      if (this.form.gender === '') {
        this.form.errors.gender = "(!) Виберіть стать";
        return false;
      } else {
        return true;
      }
    },
    validatePhone() {
      if (this.form.phone.length >= 18) {
        return true;
      } else {
        this.form.errors.phone = '(!) Введіть всі цифри в номер';
        return false;
      }
    },
    validateGroup() {
      if (!this.form.group) {
        this.form.errors.groupErr = '(!) Виберіть групу';
        return false;
      } else {
        return true;
      }
    },
    validateBirthDate() {
      const birthDate = new Date(this.form.birthDate);
      const today = new Date();
      const minBirthDate = new Date(1900, 5, 22);
      if (!this.form.birthDate) {
        this.form.errors.birthDate = '(!) Заповніть дату народження';
        return false;
      } else if (birthDate > today) {
        this.form.errors.birthDate = '(!) Дата народження не може бути у майбутньому.';
        return false;
      } else if (birthDate < minBirthDate) {
        this.form.errors.birthDate = '(!) Мінімальний рік народження - 1900';
        return false;
      } else {
        return true;
      }
    },
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

    this.form.maxDate = `${yyyy}-${mm}-${dd}`;
  },
  directives: {
    imask: IMaskDirective
  }
}
</script>

<style scoped>
.input-box, .input-gender, .input-phone, .input-date, .input-group {
  position: relative;
  margin: 30px 0 30px 380px;
  width: 310px;
  border-bottom: 2px solid #fff;
  color: white;
}

.error-message {
  color: rgb(255, 0, 0);
  position: absolute;
  top: 110%;
  left: 5px;
  font-size: 1em;
}

@media (max-width: 768px) {
  .input-box, .input-gender, .input-phone, .input-date, .input-group {
    margin: 20px 0;
  }
}
</style>