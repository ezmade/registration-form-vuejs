<template>
  <form class="registration-form" @submit.prevent="handleSubmit" novalidate>
    <div class="step" v-show="form.step === 1">
      <h1>Создание нового клиента</h1>
      <div class="grid">
        <div class="input-field" id="last-name-input-field">
          <label class="field-label">Фамилия*</label>
          <input
            type="text"
            id="last-name-input"
            class="input-field-box"
            v-model.trim="form.last_name"
            @blur="v$.form.last_name.$touch()"
          />
          <span class="error" v-if="v$.form.last_name.$error">
            Заполните обязательное поле
          </span>
        </div>
        <div class="input-field" id="first-name-input-field">
          <label class="field-label">Имя*</label>
          <input
            type="text"
            pattern="[а-яА-Я]+"
            data-error="Введите буквы"
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
      </div>
      
      <div class="grid">
        <div class="input-field" id="birthday-input-field">
          <label class="field-label">Дата рождения*</label>
          <input
            type="date"
            id="birthday-input"
            class="input-field-box"
            v-model.trim="form.birthday"
            @blur="v$.form.birthday.$touch()"
          />
          <span class="error" v-if="v$.form.birthday.$error">
            Заполните обязательное поле
          </span>
        </div>

        <div class="genders">
          <label class="field-label">Пол</label>
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
        </div>
      </div>
    
      <div class="grid">
        <div class="select-field" id="client-group-select-field">
          <label class="field-label" for="client-group-select">
            Группа клиентов*
          </label>
          <select
            id="client-group-select"
            class="select-field-box"
            v-model="form.client_group"
            multiple
            @blur="v$.form.client_group.$touch()"
          >
            <option
              v-for="(client_group, index) in client_groups"
              :value="client_group.value"
              :key="index"
            >
              {{ client_group.label }}
            </option>
          </select>
          <span class="error" v-if="v$.form.client_group.$invalid">
            Заполните обязательное поле
          </span>
        </div>

        <div class="select-field" id="doctor-select-field">
          <label class="field-label" for="doctor-select">Лечащий врач</label>
          <select
            id="doctor-select"
            class="select-field-box"
            v-model="form.doctor"
          >
            <option
              v-for="(doctor, index) in doctors"
              :value="doctor.value"
              :key="index"
            >
              {{ doctor.label }}
            </option>
          </select>
        </div>
      </div>

      <div class="grid grid-single">
        <div class="input-field" id="phone-input-field">
          <label class="field-label">Телефон*</label>
          <input
            type="tel"
            id="phone-input"
            class="input-field-box"
            placeholder="79001234567"
            pattern="7[0-9]{10}"
            v-model.trim="form.phone"
            @input="checkValidPhone"
            @blur="v$.form.phone.$touch()"
          />
          <span class="error" v-if="v$.form.phone.$error">
            Номер телефона должен содержать 11 цифр, начиная с "7"
          </span>
        </div>
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
        class="btn btn-next"
      >
        Следующий шаг
      </button>

      <span class="additional-info">* - Поле обязательное для заполнения</span>
    </div>

    <div class="step" v-show="form.step === 2">
      <h1>Адресс проживания</h1>

      <div class="grid">
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
      </div>

      <div class="grid">
        <div class="input-field" id="city-input-field">
          <label class="field-label">Город*</label>
          <input
            type="text"
            id="city-input"
            class="input-field-box"
            v-model.trim="form.city"
            @blur="v$.form.city.$touch()"
          />
          <span class="error" v-if="v$.form.city.$error">
            Заполните обязательное поле
          </span>
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
      </div>

      <button @click="previousStep()" type="button" class="btn btn-back">
        Предыдущий шаг
      </button>
      <button
        @click="nextStep()"
        :disabled="disabledBtnNextAddress"
        type="button"
        class="btn btn-next"
      >
        Следующий шаг
      </button>

      <span class="additional-info">* - Поле обязательное для заполнения</span>
    </div>

    <div class="step" v-show="form.step === 3">
      <h1>Документ, удостоверяющий личность</h1>

      <div class="grid grid-single">
        <div class="select-field" id="document-type-select-field">
          <label class="field-label" for="document-type-select">
            Тип документа*
          </label>
          <select
            id="document-type-select"
            class="select-field-box"
            v-model="form.document_type"
          >
            <option
              v-for="(document_type, index) in document_types"
              :value="document_type.value"
              :key="index"
            >
              {{ document_type.label }}
            </option>
          </select>
        </div>
      </div>

      <div class="grid">
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
      </div>
      <div class="grid">
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
            @blur="v$.form.document_date.$touch()"
          />
          <span class="error" v-if="v$.form.document_date.$error">
            Заполните обязательное поле
          </span>
        </div>
      </div>
      

      <button @click="previousStep()" type="button" class="btn btn-back">
        Предыдущий шаг
      </button>
      <button
        type="submit"
        :disabled="disabledBtnNextDocuments"
        class="btn btn-submit"
      >
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
        birthday: "",
        gender: "male",
        client_group: ["ОМС"],
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
        document_date: "",
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
        this.v$.form.client_group.$invalid ||
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
      alert("Новый клиент успешно создан.");
      return;
    },
  },

  validations() {
    return {
      form: {
        first_name: { 
          required
        },
        last_name: { 
          required
        },
        birthday: { 
          required
        },
        phone: {
          required,
          validPhone() {
            return this.form.phone.toString()[0] == 7;
          }, 
          minLength: minLength(11),
          maxLength: maxLength(11),
          integer,
        },
        client_group: { required },
        city: { required },
        document_type: { required },
        document_date: { required }
      }
    };
  },
};
</script>

<style>
h1 {
  font-size: 2.5rem;
  font-family: "Source Sans Pro", sans-serif;
  font-style: normal;
  text-transform: uppercase;
  margin-bottom: 4rem;
}

.registration-form {
  display: grid;
  align-items: center;
  min-height: 70vh;
  height: fit-content;
  width: 100rem;
  padding: 5rem 2rem;
  background-color: rgb(250, 250, 250);
  border-radius: 1rem;
  border-top-right-radius: 10rem;
  border-bottom-left-radius: 10rem;
  box-shadow: 1.5rem 2.8rem 2.8rem -0.5rem rgba(0, 0, 0, 0.3);
}

.grid {
  display: grid;
  align-items: flex-start;
  grid-template-columns: repeat(auto-fit, minmax(23rem, 1fr));
  gap: 4rem;
  margin-bottom: 2rem;
}

.grid-single {
  grid-template-columns: repeat(auto-fill, minmax(20rem, 1fr));
}

input, select {
  font-size: 2rem;
}
.input-field, .genders{
  display: grid;
}

.field-label {
  font-size: 2rem;
  font-weight: 500;
}

.input-field-box {
  padding: 1rem;
  height: 4rem;
  background: #f0f0f0;
  border: 0.5px solid rgba(0, 0, 0, 0.5);
  border-radius: 1rem;
  box-sizing: border-box;
  transition: 0.2s;
}

.input-field-box:focus {
  border-radius: 1rem;
  outline: none;
  background: rgb(196, 226, 255);
}

.select-field {
  display: grid;
}

.select-field-box {
  height: 5rem;
  padding: 1rem;
  border: 0.5px solid rgba(0, 0, 0, 0.5);
  border-radius: 1rem;
  box-sizing: border-box;
  transition: 0.2s;
}

.select-field-box:focus {
  border-radius: 1rem;
  outline: none;
  background: rgb(196, 226, 255);
}

#client-group-select {
  height: fit-content;
}

#client-group-select::-webkit-scrollbar {display:none;}
#client-group-select::-moz-scrollbar {display:none;}
#client-group-select::-o-scrollbar {display:none;}
#client-group-select::-google-ms-scrollbar {display:none;}
#client-group-select::-khtml-scrollbar {display:none;}


.check-select-field {
  display: flex;
  align-items: center;
}

.check-select-field input {
  margin-right: 0.6rem;
}

.gender-select-field {
  display: flex;
  align-items: center;
  justify-content: center;
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
  color: white;
  transition: 0.2s;
}

.btn:disabled {
  background: #ebebeb;
}

.btn-next,
.btn-submit {
  background: rgb(83, 83, 255);
}

.btn-next:hover:enabled,
.btn-submit:hover:enabled {
  background: rgb(102, 102, 252);
}

.btn-back {
  background: rgb(194, 194, 194);
}

.btn-back:hover {
  background: rgb(180, 180, 180);
}

.error {
  color: red;
}

.additional-info {
  display: flex;
  font-style: normal;
  font-size: 1.1rem;
  margin-top: 2rem;
}

</style>