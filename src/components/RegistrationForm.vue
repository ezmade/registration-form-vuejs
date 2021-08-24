<template>
    <form class="registration-form" @submit.prevent="handleSubmit">
        <div class="step" v-show="form.step === 1">
            <h1>Создание нового клиента</h1>

            <div class="input-field" id="last-name-input-field">
                <label class="field-label">Фамилия</label>
                <input type="text" id="last-name-input" class="input-field-box" v-model.trim="form.last_name">
            </div>

            <div class="input-field" id="first-name-input-field">
                <label class="field-label">Имя</label>
                <input type="text" id="first-name-input" class="input-field-box" v-model.trim="form.first_name">
            </div>

            <div class="input-field" id="third-name-input-field">
                <label class="field-label">Отчество</label>
                <input type="text" id="third-name-input" class="input-field-box" v-model.trim="form.third_name">
            </div>

            <div class="input-field" id="birthday-input-field">
                <label class="field-label">Дата рождения</label>
                <input type="date" id="birthday-input" class="input-field-box" v-model.trim="form.birthday">
                <p class="invalid-feedbalck">Ошибка !</p>
            </div>

            <div class="radio-select-field" id="gender-select-field">
                <div class="select-field">
                    <input type="radio" value="male" class="check-input" id="male-select" v-model="form.gender">
                    <label class="field-label" for="male-select">Мужчина</label>
                </div>
                <div class="select-field">
                    <input type="radio" value="female" class="check-input" id="female-select" v-model="form.gender">
                    <label class="field-label" for="female-select">Женщина</label>
                </div>
            </div>

            <div class="select-field" id=client-group-select-field>
                <label class="field-label" for="client-group-select">Группа клиентов</label>
                <select id="client-group-select" class="select-field-box" v-model="form.clientGroup" multiple>
                    <option 
                    v-for="(client_group, index) in client_groups" 
                    :value="client_group.value"
                    :key="index">
                        {{ client_group.label }}
                    </option>
                </select>
            </div>

            <div class="select-field" id=doctor-select-field>
                <label class="field-label" for="doctor-select">Лечащий врач</label>
                <select id="doctor-select" class="select-field-box" v-model="form.doctor">
                    <option 
                    v-for="(doctor, index) in doctors" 
                    :value="doctor.value"
                    :key="index">
                        {{ doctor.label }}
                    </option>
                </select>
            </div>

            <div class="input-field" id="phone-input-field">
                <label class="field-label">Телефон</label>
                <input type="number" id="phone-input" class="input-field-box" v-model.trim="form.phone">
                <p class="invalid-feedbalck">Ошибка !</p>
            </div>
            
            <div class="input-field" id="check-input-field">
                <input type="checkbox" class="check-input" id="send-message" v-model="form.dontSendMessage">
                <label class="field-label" for="send-message">Не отправлять СМС</label> 
            </div>
        
            <button @click="nextStep()" type="button" class="btn">Следующий шаг</button>
        </div>

        <div class="step" v-show="form.step === 2">
            <h1>Создание нового клиента</h1>

            <div class="input-field" id="index-input-field">
                <label class="field-label">Индекс</label>
                <input type="text" id="index-input" class="input-field-box" v-model.trim="form.address_index">
            </div>

            <div class="input-field" id="country-input-field">
                <label class="field-label">Страна</label>
                <input type="text" id="country-input" class="input-field-box" v-model.trim="form.country">
            </div>

            <div class="input-field" id="region-input-field">
                <label class="field-label">Область</label>
                <input type="text" id="region-input" class="input-field-box" v-model.trim="form.region">
            </div>

            <div class="input-field" id="city-input-field">
                <label class="field-label">Город</label>
                <input type="text" id="city-input" class="input-field-box" v-model.trim="form.city">
            </div>

           <div class="input-field" id="street-input-field">
                <label class="field-label">Улица</label>
                <input type="text" id="street-input" class="input-field-box" v-model.trim="form.street">
            </div>

            <div class="input-field" id="home-input-field">
                <label class="field-label">Дом</label>
                <input type="text" id="home-input" class="input-field-box" v-model.trim="form.home">
            </div>
            
            <button @click="previousStep()" type="button" class="btn">Предыдущий шаг</button>
            <button @click="nextStep()" type="button" class="btn">Следующий шаг</button>
        </div>

        <div class="step" v-show="form.step === 3">
            <h1>Создание нового клиента</h1>

            <div class="select-field" id=document-type-select-field>
                <label class="field-label" for="document-type-select">Тип документа</label>
                <select id="document-type-select" class="select-field-box" v-model="form.document_type">
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
                <input type="text" id="document-serial-input" class="input-field-box" v-model.trim="form.document_serial">
            </div>

            <div class="input-field" id="document-number-field">
                <label class="field-label">Номер</label>
                <input type="text" id="document-number-input" class="input-field-box" v-model.trim="form.document_number">
            </div>

            <div class="input-field" id="document-organization-field">
                <label class="field-label">Кем выдан</label>
                <input type="text" id="document-organization-input" class="input-field-box" v-model.trim="form.document_organization">
            </div>

            <div class="input-field" id="document-date-field">
                <label class="field-label">Дата выдачи</label>
                <input type="date" id="document-date-input" class="input-field-box" v-model.trim="form.document_date">
                <p class="invalid-feedbalck">Ошибка !</p>
            </div>
            
            <button @click="previousStep()" type="button" class="btn">Предыдущий шаг</button>
            <button type="submit" class="btn">Завершить</button>
        </div>
    </form> 
</template>

<script>

    export default {
        name: 'RegistrationForm',
        data() {
            return {
                form: {
                    step: 1,
                    first_name: '',
                    last_name: '',
                    third_name: '',
                    birthday: '01.01.1970',
                    gender: 'male',
                    clientGroup: ['ОМС'],
                    doctor: 'Иванов',
                    dontSendMessage: false,
                    address_index: '',
                    country: '',
                    region: '',
                    city: '',
                    street: '',
                    home: '',
                    document_type: 'Паспорт',
                    document_serial: '',
                    document_number: '',
                    document_organization: '',
                    document_date: '01.01.1970'
                },

                doctors: [
                    {
                        label: 'Иванов',
                        value: 'Иванов'
                    },
                    {
                        label: 'Захаров',
                        value: 'Захаров'
                    },
                    {
                        label: 'Чернышева',
                        value: 'Чернышева'
                    }
                ],
                client_groups: [
                    {
                        label: 'VIP',
                        value: 'VIP'
                    },
                    {
                        label: 'Проблемные',
                        value: 'Проблемные'
                    },
                    {
                        label: 'ОМС',
                        value: 'ОМС'
                    }
                ],
                document_types: [
                    {
                        label: 'Паспорт',
                        value: 'Паспорт'
                    },
                    {
                        label: 'Свидетельство о рождении',
                        value: 'Свидетельство о рождении'
                    },
                    {
                        label: 'Вод. удостоверение',
                        value: 'Вод. удостоверение'
                    }
                ]
            }
        },

        methods: {
            previousStep() {
                this.form.step--;
            },

            nextStep() {
                this.form.step++;
            },

            handleSubmit() {
                console.log('Submitted');
            }
        },
    }
</script>