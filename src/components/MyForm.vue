<template>
  <form @submit.prevent="submitForm">
    <div>
      <label for="surname">Фамилия*</label>
      <input id="surname" v-model.trim="formData.surname" required />
    </div>
    <div>
      <label for="name">Имя*</label>
      <input id="name" v-model.trim="formData.name" required />
    </div>
    <div>
      <label for="patronymic">Отчество</label>
      <input id="patronymic" v-model.trim="formData.patronymic" />
    </div>
    <div>
      <label for="birthdate">Дата рождения*</label>
      <input id="birthdate" type="date" v-model="formData.birthdate" required />
    </div>
    <div>
      <label for="phone">Номер телефона*</label>
      <input
        id="phone"
        v-model.trim="formData.phone"
        required
        pattern="7\d{10}"
      />
    </div>
    <div>
      <label>Пол</label>
      <input type="radio" id="male" value="male" v-model="formData.gender" />
      <label for="male">Мужской</label>
      <input
        type="radio"
        id="female"
        value="female"
        v-model="formData.gender"
      />
      <label for="female">Женский</label>
    </div>
    <div>
      <label for="clientGroup">Группа клиентов*</label>
      <select id="clientGroup" v-model="formData.clientGroup" multiple required>
        <option value="VIP">VIP</option>
        <option value="Проблемные">Проблемные</option>
        <option value="ОМС">ОМС</option>
      </select>
    </div>
    <div>
      <label for="doctor">Лечащий врач</label>
      <select id="doctor" v-model="formData.doctor">
        <option value="Иванов">Иванов</option>
        <option value="Захаров">Захаров</option>
        <option value="Чернышева">Чернышева</option>
      </select>
    </div>
    <div>
      <input type="checkbox" id="noSMS" v-model="formData.noSMS" />
      <label for="noSMS">Не отправлять СМС</label>
    </div>
    <div>
      <h2>Адрес</h2>
      <label for="index">Индекс</label>
      <input id="index" v-model.trim="formData.address.index" />
      <label for="country">Страна</label>
      <input id="country" v-model.trim="formData.address.country" />
      <label for="region">Область</label>
      <input id="region" v-model.trim="formData.address.region" />
      <label for="city">Город*</label>
      <input id="city" v-model.trim="formData.address.city" required />
      <label for="street">Улица</label>
      <input id="street" v-model.trim="formData.address.street" />
      <label for="house">Дом</label>
      <input id="house" v-model.trim="formData.address.house" />
    </div>
    <div>
      <h2>Паспорт</h2>
      <label for="docType">Тип документа*</label>
      <select id="docType" v-model="formData.passport.type" required>
        <option value="Паспорт">Паспорт</option>
        <option value="Свидетельство о рождении">
          Свидетельство о рождении
        </option>
        <option value="Вод. удостоверение">Вод. удостоверение</option>
      </select>
      <label for="passportSeries">Серия</label>
      <input id="passportSeries" v-model.trim="formData.passport.series" />
      <label for="passportNumber">Номер</label>
      <input id="passportNumber" v-model.trim="formData.passport.number" />
      <label for="passportIssuedBy">Кем выдан</label>
      <input id="passportIssuedBy" v-model.trim="formData.passport.issuedBy" />
      <label for="passportIssueDate">Дата выдачи*</label>
      <input
        id="passportIssueDate"
        type="date"
        v-model="formData.passport.issueDate"
        required
      />
    </div>
    <button v-on:click="submitForm" type="submit">Отправить</button>
  </form>
</template>

<script>
import { required, minLength, maxLength } from "@vuelidate/validators";

export default {
  data() {
    return {
      formData: {
        surname: "",
        name: "",
        patronymic: "",
        birthdate: "",
        phone: "",
        gender: "",
        clientGroup: [],
        doctor: "",
        noSMS: false,
        address: {
          index: "",
          country: "",
          region: "",
          city: "",
          street: "",
          house: "",
        },
        passport: {
          type: "",
          series: "",
          number: "",
          issuedBy: "",
          issueDate: "",
        },
      },
    };
  },
  methods: {
    submitForm() {
      if (
        this.formData.name &&
        this.formData.surname &&
        this.formData.phone &&
        this.formData.clientGroup.length &&
        this.formData.address.city &&
        this.formData.phone.startsWith("7") &&
        this.formData.phone.length === 11 &&
        this.formData.passport.type
      ) {
        alert("клиент успешно создан");
        this.formData = {
          surname: "",
          name: "",
          patronymic: "",
          birthdate: "",
          phone: "",
          gender: "",
          clientGroup: [],
          doctor: "",
          noSMS: false,
          address: {
            index: "",
            country: "",
            region: "",
            city: "",
            street: "",
            house: "",
          },
          passport: {
            type: "",
            series: "",
            number: "",
            issuedBy: "",
            issueDate: "",
          },
        };
      }
    },
  },
  validations: {
    formData: {
      surname: {
        required,
      },
      name: {
        required,
      },
      birthdate: {
        required,
      },
      phone: {
        required,
        minLength: 11,
        maxLength: 11,
        startsWith7: (value) => {
          return value && value.startsWith("7");
        },
      },
      clientGroup: {
        required,
      },
      address: {
        city: {
          required,
        },
      },
      passport: {
        type: {
          required,
        },
        issueDate: {
          required,
        },
      },
    },
  },
};
</script>

<style lang="scss">
form {
  display: flex;
  flex-direction: column;
  max-width: 400px;
  margin: auto;
}

div {
  margin-bottom: 15px;
}

label {
  font-weight: bold;
}

input[type="text"],
input[type="date"],
select,
button {
  width: 100%;
  padding: 8px;
  border: 1px solid #ccc;
  border-radius: 5px;
}

input[type="checkbox"],
input[type="radio"] {
  margin-right: 5px;
}

// Custom styling for specific elements
h2 {
  margin-top: 20px;
  margin-bottom: 10px;
}

#clientGroup,
#doctor,
#docType {
  height: 100px; /* Adjust height for multi-select */
}

button {
  background-color: #007bff;
  color: #fff;
  border: none;
  cursor: pointer;
  padding: 10px;
  transition: background-color 0.3s ease;
}

button:hover {
  background-color: #0056b3;
}

// Custom styling for required fields
input:required {
  border-color: red;
}

// Media query for responsiveness
@media screen and (max-width: 600px) {
  form {
    max-width: 100%;
  }
}
</style>
