<template>
  <form class="registration-form" @submit.prevent="handleSubmit">
    <div class="step" v-show="form.step === 1">
      <h1>Создание нового клиента</h1>

      <div class="input-field" id="last-name-input-field">
        <label class="field-label">Фамилия*</label>
        <input
          type="text"
          id="last-name-input"
          class="input-field-box"
          v-model.trim="form.last_name"
          @blur="v$.form.last_name.$touch()"
        />
        <p class="error" v-if="v$.form.last_name.$error">
          Заполните обязательное поле
        </p>
      </div>

      <div class="input-field" id="first-name-input-field">
        <label class="field-label">Имя*</label>
        <input
          type="text"
          id="first-name-input"
          class="input-field-box"
          v-model.trim="form.first_name"
          @blur="v$.form.first_name.$touch()"
        />
        <div class="error" v-if="v$.form.first_name.$error">
          Заполните обязательное поле
        </div>
      </div>

      <div class="input-field" id="third-name-input-field">
        <label class="field-label">Отчество</label>
        <input
          type="text"
          id="third-name-input"
          class="input-field-box"
          v-model.trim="form.third_name"
        />
      </div>

      <div class="input-field" id="birthday-input-field">
        <label class="field-label">Дата рождения*</label>
        <input
          type="date"
          id="birthday-input"
          class="input-field-box"
          v-model.trim="form.birthday"
          @blur="v$.form.birthday.$touch()"
        />
        <p class="error" v-if="v$.form.birthday.$error">
          Заполните обязательное поле
        </p>
      </div>

      <div class="check-select-field" id="gender-radio-select-field">
        <div class="gender-select-field">
          <input
            type="radio"
            value="male"
            class="check-input"
            id="male-select"
            v-model="form.gender"
          />
          <label class="field-label" for="male-select">Мужчина</label>
        </div>
        <div class="gender-select-field">
          <input
            type="radio"
            value="female"
            class="check-input"
            id="female-select"
            v-model="form.gender"
          />
          <label class="field-label" for="female-select">Женщина</label>
        </div>
      </div>

      <div class="select-field" id="client-group-select-field">
        <label class="field-label" for="client-group-select">
          Группа клиентов*
        </label>
        <select
          id="client-group-select"
          class="select-field-box"
          v-model="form.clientGroup"
          multiple
          @blur="v$.form.client_group.$touch()">
          <option
            v-for="(client_group, index) in client_groups"
            :value="client_group.value"
            :key="index">
            {{ client_group.label }}
          </option>
        </select>
        <!-- <p class="error" v-if="v$.form.client_group.$error">
          Заполните обязательное поле
        </p> -->
      </div>

      <div class="select-field" id="doctor-select-field">
        <label class="field-label" for="doctor-select">Лечащий врач</label>
        <select
          id="doctor-select"
          class="select-field-box"
          v-model="form.doctor">
          <option
            v-for="(doctor, index) in doctors"
            :value="doctor.value"
            :key="index">
            {{ doctor.label }}
          </option>
        </select>
      </div>

      <div class="input-field" id="phone-input-field">
        <label class="field-label">Телефон*</label>
        <input
          type="tel"
          id="phone-input"
          class="input-field-box"
          placeholder="89001234567"
          pattern="89[0-9]{9}"
          v-model.trim="form.phone"
          @blur="v$.form.phone.$touch()"
        />
        <p class="error" v-if="v$.form.phone.$error">
          Номер телефона должен содержать 11 цифр
        </p>
      </div>

      <div class="check-select-field">
        <input
          type="checkbox"
          class="check-input"
          id="send-message"
          v-model="form.dontSendMessage"
        />
        <label class="field-label" for="send-message">Не отправлять СМС</label>
      </div>

      <button
        @click="nextStep()"
        :disabled="disabledBtnNextInfo"
        type="button"
        class="btn btn-next">
        Следующий шаг
      </button>

      <span class="additional-info">* - Поле обязательное для заполнения</span>

    </div>

    <div class="step" v-show="form.step === 2">
      <h1>Создание нового клиента</h1>

      <div class="input-field" id="index-input-field">
        <label class="field-label">Индекс</label>
        <input
          type="text"
          id="index-input"
          class="input-field-box"
          v-model.trim="form.address_index"
        />
      </div>

      <div class="input-field" id="country-input-field">
        <label class="field-label">Страна</label>
        <input
          type="text"
          id="country-input"
          class="input-field-box"
          v-model.trim="form.country"
        />
      </div>

      <div class="input-field" id="region-input-field">
        <label class="field-label">Область</label>
        <input
          type="text"
          id="region-input"
          class="input-field-box"
          v-model.trim="form.region"
        />
      </div>

      <div class="input-field" id="city-input-field">
        <label class="field-label">Город*</label>
        <input
          type="text"
          id="city-input"
          class="input-field-box"
          v-model.trim="form.city"
        />
      </div>

      <div class="input-field" id="street-input-field">
        <label class="field-label">Улица</label>
        <input
          type="text"
          id="street-input"
          class="input-field-box"
          v-model.trim="form.street"
        />
      </div>

      <div class="input-field" id="home-input-field">
        <label class="field-label">Дом</label>
        <input
          type="text"
          id="home-input"
          class="input-field-box"
          v-model.trim="form.home"
        />
      </div>

      <button @click="previousStep()" type="button" class="btn btn-back">
        Предыдущий шаг
      </button>
      <button
        @click="nextStep()"
        :disabled="disabledBtnNextAddress"
        type="button"
        class="btn btn-next">
        Следующий шаг
      </button>

      <span class="additional-info">* - Поле обязательное для заполнения</span>

    </div>

    <div class="step" v-show="form.step === 3">
      <h1>Создание нового клиента</h1>

      <div class="select-field" id="document-type-select-field">
        <label class="field-label" for="document-type-select">
          Тип документа*
        </label>
        <select
          id="document-type-select"
          class="select-field-box"
          v-model="form.document_type">
          <option
            v-for="(document_type, index) in document_types"
            :value="document_type.value"
            :key="index">
            {{ document_type.label }}
          </option>
        </select>
      </div>

      <div class="input-field" id="document-serial-input-field">
        <label class="field-label">Серия</label>
        <input
          type="text"
          id="document-serial-input"
          class="input-field-box"
          v-model.trim="form.document_serial"
        />
      </div>

      <div class="input-field" id="document-number-field">
        <label class="field-label">Номер</label>
        <input
          type="text"
          id="document-number-input"
          class="input-field-box"
          v-model.trim="form.document_number"
        />
      </div>

      <div class="input-field" id="document-organization-field">
        <label class="field-label">Кем выдан</label>
        <input
          type="text"
          id="document-organization-input"
          class="input-field-box"
          v-model.trim="form.document_organization"
        />
      </div>

      <div class="input-field" id="document-date-field">
        <label class="field-label">Дата выдачи*</label>
        <input
          type="date"
          id="document-date-input"
          class="input-field-box"
          v-model.trim="form.document_date"
        />
      </div>

      <button @click="previousStep()" type="button" class="btn btn-back">
        Предыдущий шаг
      </button>
      <button type="submit" :disabled="disabledBtnNextDocuments" class="btn btn-submit">
        Завершить
      </button>

      <span class="additional-info">* - Поле обязательное для заполнения</span>
    </div>
  </form>
</template>

<script>
import { useVuelidate } from "@vuelidate/core";
import { required, maxLength, minLength, integer } from "@vuelidate/validators";

export default {
  setup() {
    return { v$: useVuelidate() };
  },
  data() {
    return {
      form: {
        step: 1,
        first_name: "",
        last_name: "",
        third_name: "",
        birthday: "1970-01-01",
        gender: "male",
        clientGroup: ["ОМС"],
        doctor: "Иванов",
        phone: "",
        dontSendMessage: false,
        address_index: "",
        country: "",
        region: "",
        city: "",
        street: "",
        home: "",
        document_type: "Паспорт",
        document_serial: "",
        document_number: "",
        document_organization: "",
        document_date: "1970-01-01",
      },

      doctors: [
        {
          label: "Иванов",
          value: "Иванов",
        },
        {
          label: "Захаров",
          value: "Захаров",
        },
        {
          label: "Чернышева",
          value: "Чернышева",
        },
      ],
      client_groups: [
        {
          label: "VIP",
          value: "VIP",
        },
        {
          label: "Проблемные",
          value: "Проблемные",
        },
        {
          label: "ОМС",
          value: "ОМС",
        },
      ],
      document_types: [
        {
          label: "Паспорт",
          value: "Паспорт",
        },
        {
          label: "Свидетельство о рождении",
          value: "Свидетельство о рождении",
        },
        {
          label: "Вод. удостоверение",
          value: "Вод. удостоверение",
        },
      ],
    };
  },

  computed: {
    disabledBtnNextInfo() {
      return (
        this.v$.form.first_name.$invalid ||
        this.v$.form.last_name.$invalid ||
        this.v$.form.birthday.$invalid ||
        //    this.v$.form.client_group.$invalid ||
        this.v$.form.phone.$invalid
      );
    },

    disabledBtnNextAddress() {
      return this.v$.form.city.$invalid;
    },

    disabledBtnNextDocuments() {
      return (
        this.v$.form.document_type.$invalid ||
        this.v$.form.document_date.$invalid
      );
    },
  },

  methods: {
    previousStep() {
      this.form.step--;
    },

    nextStep() {
      this.form.step++;
    },

    handleSubmit() {
      alert("Пользователь успешно создан!");
      return
    },
  },

  validations() {
    return {
      form: {
        first_name: { required },
        last_name: { required },
        birthday: { required },
        phone: {
          required,
          minLength: minLength(11),
          maxLength: maxLength(11),
          integer,
        },
        client_group: { required },
        city: { required },
        document_type: { required },
        document_date: { required },
      },
    };
  },
};
</script>

<style>

h1 {
  font-size: 3rem;
  font-family: 'Source Sans Pro', sans-serif;
  font-style: normal;
}

.registration-form {
  display: flex;
  justify-content: center;
  min-height: 70vh;
  height: fit-content;
  width: 80vw;
  padding: 5rem 2rem;
  background-color: rgb(250, 250, 250);
  border-radius: 1.5rem;
}

.input-field {
  display: grid;
}

.field-label {
  font-size: 1.8rem;
  font-weight: 500;
}

.input-field-box {
  padding: 1rem;
  height: 4rem;
  background: #F0F0F0;
  border: 0.5px solid rgba(0, 0, 0, 0.5);
  border-radius: 1rem;
  box-sizing: border-box;

}

.select-field {
  display: grid;
}

.select-field-box {
  padding: 1rem;
  border: 0.5px solid rgba(0, 0, 0, 0.5);
  border-radius: 1rem;
  box-sizing: border-box;
}

.check-select-field {
  display: flex;
  align-items: center;
}

.check-select-field input {
  margin-right: .6rem;
}

.gender-select-field {
  display: flex;
  align-items: center;
  padding-right: 2rem;
}

.btn {
  text-align: center;
  padding: 0 1rem;
  margin-top: 1rem;
  margin-right: 1rem;
  height: 4rem;
  border-radius: 1rem;
  font-size: 1.5rem;
  border: 0;
}

.btn-next, .btn-submit {
  background: rgb(83, 83, 255);
  color: white;
}

.btn-back {
  background: rgb(194, 194, 194);
}

.error {
  color: red;
}

.additional-info {
  display: flex;
  font-style: italic;
  font-size: 1.1rem;
  margin-top: 2rem;
}
</style>