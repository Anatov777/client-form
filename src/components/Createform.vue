<template>
  <form class="create-client-form" @submit.prevent="checkForm">
    <div class="create-client-form__content">
      <div class="create-client-form__main">
        <h2>Основные данные</h2>
        <div class="create-client-form__item">
          <label for="firstName">Имя:<span class="star">*</span></label>
            <input
              id="firstName"
              type="text"
              v-model.trim="name"
              :class="$v.name.$error ? 'is-invalid' : ''"
            > 
        </div>
        <p v-if="$v.name.$dirty && !$v.name.required" class="invalid-feedback">
          Обязательное поле
        </p>

        <div class="create-client-form__item">
          <label for="lastName">Фамилия:<span class="star">*</span></label>
          <input
            id="lastName"
            type="text"
            v-model.trim="lastname"
            :class="$v.lastname.$error ? 'is-invalid' : ''"
          >
        </div>
        <p v-if="$v.lastname.$dirty && !$v.lastname.required" class="invalid-feedback">
          Обязательное поле
        </p>

        <div class="create-client-form__item">
          <label for="patronymic">Отчество:</label>
          <input
            id="patronymic"
            type="text"
          >
        </div>

        <div class="create-client-form__item">
          <label for="birthdate">Дата рождения:<span class="star">*</span></label>
          <input
            id="birthdate"
            class="birthdate"
            type="date"
            v-model.trim="birthdate"
            :class="$v.birthdate.$error ? 'is-invalid' : ''"
            :max="getNowDate()"
          >
        </div>
        <p v-if="$v.birthdate.$dirty && !$v.birthdate.required" class="invalid-feedback">
          Обязательное поле
        </p>

        <div class="create-client-form__item"> 
          <label for="phone">Номер телефона:<span class="star">*</span></label>
          <input
            id="phone"
            class="phone"
            type="tel"
            maxlength="11"
            v-model.trim="phone"
            :class="$v.phone.$error ? 'is-invalid' : ''"
          >
        </div>
        <p v-if="$v.phone.$dirty && !$v.phone.required" class="invalid-feedback">
          Обязательное поле <span v-if="$v.phone.$dirty && !$v.phone.minLength" class="invalid-feedback">
          Номер состоит из 11 цифр</span>
        </p>
        <p v-if="$v.phone.$dirty && !$v.phone.minLength" class="invalid-feedback">
          Номер состоит из 11 цифр
        </p>
        <p p v-if="$v.phone.$dirty && !$v.phone.validPhone" class="invalid-feedback">
          Номер начинается с цифры 7 (формат записи: 7123456789)
        </p>
    

        <div class="gender-clientgroup">
          <div class="gender-input">
            <div class="gender-title">Пол:</div>
            <div class="gender-input__item">
              <input id="male" type="radio" name="gender">
              <label for="male">Мужчина</label>
            </div>
            <div class="gender-input__item">
              <input id="female" type="radio" name="gender">
              <label for="female">Женщина</label>
            </div>
          </div>
          <div class="clientgroup-input">
            <label for="clientgroup" class="clientgroup-title">Группа клиентов:<span class="star">*</span></label>
            <div id="clientgroup" class="select-pass">
              <div id="chooseGroup" class="select-pass__select" @click="selGroupCliked = !selGroupCliked">
                {{ clientGroup.length > 0 ? clientGroup.join(', ') : 'Выберите...' }}
              </div>
              <div class="select-pass__options" v-if="selGroupCliked">
                <div
                  class="option-container"
                  v-for="(group, index) in groups"
                  :key="index"
                >
                  <input
                    type="checkbox"
                    :id="'group'+(index+1)"
                    :value="group.value"
                    v-model="clientGroup"
                    @checked="selectOption()"
                  >
                  <label :for="'group'+(index+1)">
                    {{ group.label }}
                  </label>
                </div>
                <div class="ok-btn"><span @click="selGroupCliked = !selGroupCliked">ОК</span></div>
              </div>
            </div>
          </div>
        </div>
        <p v-if="$v.clientGroup.$dirty && !$v.clientGroup.required" class="invalid-feedback invalid-client">
          Обязательное поле
        </p>

        <div class="create-client-form__item">
          <label for="attendingdoctor">Лечащий врач:<span class="star">*</span></label>
          <div id="attendingdoctor" class="select-pass">
            <div id="chooseDoctor" class="select-pass__select" @click="selDoctorCliked = !selDoctorCliked">Выберите...</div>
            <div class="select-pass__options" v-if="selDoctorCliked">
              <div
                class="select-pass__option"
                v-on:click="selectOption('chooseDoctor')"
                v-for="(doctor, index) in doctors"
                :value="doctor.value"
                :key="index"
              >
                {{ doctor.label }}
              </div>
            </div>
          </div>
        </div>
        <p v-if="$v.attendingDoctor.$dirty && !$v.attendingDoctor.required" class="invalid-feedback">
          Обязательное поле
        </p>

        <div class="sms">
          <input type="checkbox" class="form-check-input" id="notification" v-model="agreeWithSendSms">
          <label class="form-check-label" for="notification">Не отправлять СМС</label>
        </div>
      </div>

      <div class="create-client-form__address">
        <h2>Адрес</h2>
        <div class="create-client-form__item">
          <label for="city">Город:<span class="star">*</span></label>
          <input
            id="city"
            type="text"
            v-model.trim="city"
            :class="$v.city.$error ? 'is-invalid' : ''"
          >
        </div>
        <p v-if="$v.city.$dirty && !$v.city.required" class="invalid-feedback">
          Обязательное поле
        </p>
        <div class="create-client-form__item">
          <label for="zipcode">Индекс:</label>
          <input
            id="zipcode"
            type="text"
          >
        </div>
        <div class="create-client-form__item">
          <label for="country">Страна:</label>
          <input
            id="country"
            type="text"
          >
        </div>
        <div class="create-client-form__item">
          <label for="area">Область:</label>
          <input
            id="area"
            type="text"
          >
        </div>
        <div class="create-client-form__item">
          <label for="street">Улица:</label>
          <input
            id="street"
            type="text"
          >
        </div>
        <div class="create-client-form__item">
          <label for="home">Дом:</label>
          <input
            id="home"
            type="text"
          >
        </div>
      </div>
      
      <div class="create-client-form__pass">
        <h2>Паспорт</h2>
        <div class="create-client-form__item">
          <label for="passport">Тип документа:<span class="star">*</span></label>
          <div id="passport" class="select-pass">
            <div id="choosePass" class="select-pass__select" @click="selPassCliked = !selPassCliked">Выберите...</div>
            <div class="select-pass__options" v-if="selPassCliked">
              <div
                class="select-pass__option"
                v-on:click="selectOption('choosePass')"
                v-for="(document, index) in documents"
                :value="document.value"
                :key="index"
              >
                {{ document.label }}
              </div>
            </div>
          </div>
        </div>
        
        <p v-if="$v.identification.$dirty && !$v.identification.required" class="invalid-feedback">
          Обязательное поле
        </p>

        <div class="create-client-form__item">
          <label for="series">Серия:</label>
          <input
            id="series"
            type="text"
          >
        </div>
        <div class="create-client-form__item">
          <label for="passNum">Номер:</label>
          <input
            id="passNum"
            type="text"
          >
        </div>
        <div class="create-client-form__item">
          <label for="issue">Кем выдан:</label>
          <input
            id="issue"
            type="text"
          >
        </div>
        <div class="create-client-form__item">
          <label for="issueDate">Дата выдачи:<span class="star">*</span></label>
          <input
            id="issueDate"
            class="issueDate"
            type="date"
            v-model.trim="issueDate"
            :class="$v.issueDate.$error ? 'is-invalid' : ''"
            :max="getNowDate()"
          >
        </div>
        <p v-if="$v.issueDate.$dirty && !$v.issueDate.required" class="invalid-feedback">
          Обязательное поле
        </p>
      </div>
    </div>
    <div class="note"><span class="star">*</span>Поле обязательное для заполнения</div>
    <button type="submit">Создать</button>
    <div v-if="successMsg" class="success-msg">
      Клиент успешно создан!
    </div>
  </form>
</template>

<script>
import {required, minLength} from 'vuelidate/lib/validators'

export default {
  data() {
    return {
      name: '',
      lastname: '',
      birthdate: '',
      phone: '',
      clientGroup: [],
      attendingDoctor: [],
      agreeWithSendSms: false,
      city: '',
      identification: [],
      issueDate: '',
      successMsg: false,
      selGroupCliked: false,
      selPassCliked: false,
      selDoctorCliked: false,
      groups: [
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
      doctors: [
        {
          label: 'Иванов',
          value: 'Ivanov'
        },
        {
          label: 'Захаров',
          value: 'Zaharov'
        },
        {
          label: 'Чернышева',
          value: 'Chernisheva'
        }
      ],
      documents: [
        {
          label: 'Паспорт',
          value: 'Passport'
        },
        {
          label: 'Свидетельство о рождении',
          value: 'BC'
        },
        {
          label: 'Вод. удостоверение',
          value: 'driversLicense'
        }
      ],
    }
  },
  created: function() {
    let self = this;
    window.addEventListener('click', function(){
      if (!document.getElementById('clientgroup').contains(event.target)){
        self.selGroupCliked = false;
      }
      if (!document.getElementById('attendingdoctor').contains(event.target)){
        self.selDoctorCliked = false;
      }
      if (!document.getElementById('passport').contains(event.target)){
        self.selPassCliked = false;
      }
    })
  },
  validations: {
    name: {required},
    lastname: {required},
    birthdate: {required},
    phone: {
      required,
      minLength: minLength(11),
      validPhone: val => /^7\d{10}$/.test(val),
    },
    clientGroup: {required},
    attendingDoctor: {required},
    city: {required},
    identification: {required},
    issueDate: {required},
  },
  methods: {
    checkForm() {
      this.$v.$touch()
      if (!this.$v.$error) {
        console.log('Валидация прошла успешно')
        this.successMsg = true
        setTimeout(() => { this.successMsg = false }, 3000);
      }
    },
    showList() {
      alert('s');
      if (event.target.id != "chooseDoctor") {
        this.selDoctorCliked = !this.selDoctorCliked;
      }
      else if (event.target.id != "choosePass") {
        this.selPassCliked = !this.selPassCliked;
      }
    },
    selectOption(selectId) {
      if (selectId === "choosePass") {
        document.querySelector("#choosePass").innerHTML = event.target.innerHTML;
        this.selPassCliked = !this.selPassCliked;
        this.identification.push(event.target.innerHTML);
      }
      else if (selectId === "chooseDoctor") {
        document.querySelector("#chooseDoctor").innerHTML = event.target.innerHTML;
        this.selDoctorCliked = !this.selDoctorCliked;
        this.attendingDoctor.push(event.target.innerHTML);
      }     
    },
    getNowDate() {
      let date = new Date();
      let res = date.getFullYear() + '-' + ('0' + (date.getMonth() + 1)).slice(-2) + '-' + ('0' + date.getDate()).slice(-2);
      return res;
    }
  }
}
</script>