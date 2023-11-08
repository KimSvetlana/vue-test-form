<template>
    <div class="form-wrapper">
        <form class='form-group' v-bind:class="{ hide: isComplete }" id="app" @submit="sendForm"
            action="api-endpoint.com/endpoint" method="post">
            <h1 class="title">Регистрация</h1>
            <hr>
            <div class='input-group'>
                <h3 class="input-group-title">Заполните Ваши данные</h3>
                <div class='input-group-wrapper'>
                    <div class='input-group-line'>
                        <div>
                            <input class="form-control" v-bind:class="{ error: $v.form.username.$error }" type="text"
                                id="name-input" placeholder="Имя" v-model.trim="$v.form.username.$model">
                            <p class="error-message" v-if="$v.form.username.$error">Введите имя</p>
                        </div>
                        <div>
                            <input class="form-control" v-bind:class="{ error: $v.form.email.$error }" type="email"
                                id="email-input" placeholder="Email" v-model="$v.form.email.$model">
                            <p class="error-message" v-if="$v.form.email.$error">Введите email по типу example@ex.ex </p>
                        </div>
                    </div>
                    <div class='input-group-select'>
                        <div>
                            <select class="form-select" v-bind:class="{ error: $v.form.role.$error }" v-model="$v.form.role.$model">
                                <option selected>Должность</option>
                                <option value=1>Frontend developer</option>
                                <option value=2>Backend developer</option>
                                <option value=3>FullStack developer</option>
                            </select>
                            <p class="error-message" v-if="$v.form.role.$error">Выберите одну из должностей</p>
                        </div>
                    </div>
                    <div class='input-group-line'>
                        <div>
                            <input class="form-control" v-bind:class="{ error: $v.form.password.$error }" type="password"
                                id="password-input" placeholder="Пароль" v-model="$v.form.password.$model">
                            <p class="error-message" v-if="$v.form.password.$error">Введите пароль</p>
                        </div>
                        <div>
                            <input class="form-control" v-bind:class="{ error: $v.form.passwordRepeat.$error }"
                                type="password" id="password-input-1" placeholder="Повторите пароль"
                                v-model="$v.form.passwordRepeat.$model">
                            <p class="error-message" v-if="$v.form.passwordRepeat.$error">Введенные пароли не совпадают</p>
                        </div>
                    </div>
                </div>
            </div>
            <hr>
            <div class="form-check form-switch">
                <input class="form-check-input" type="checkbox" id="switch" checked v-model="form.public">
                <label class="form-check-label" for="switch">
                    <p class="switch-label">Хотите чтобы Ваш профиль видели другие участники платформы?</p>
                    <p class="switch-label-small"> Включает профиль для просмотра другими пользователями по ссылке</p>
                </label>
            </div>
            <div class="button-group">
                <div class="form-check check-wrapper">
                    <input class="form-check-input" type="checkbox" id="checkbox" checked v-model="form.privacy"
                        @input="checkboxChange($v)">
                    <label class="form-check-label" for="checkbox">
                        <p class="checkbox-label">Регистрируясь, Вы соглашаетесь с <a class="checkbox-label-link"
                                href="#">политикой конфиденциальности</a> и обработкой <a class="checkbox-label-link"
                                href="#">персональных данных</a></p>
                    </label>
                </div>
                <button class="btn btn-primary submit-button" v-bind:class="{ disabled: $v.validationGroup.$invalid }"
                    type="submit">Зарегистрироваться</button>

            </div>
        </form>
        <div class='complete-group' v-bind:class="{ visible: isComplete }">
            <p>Регистрация успешно завершена</p>
        </div>
    </div>
</template>
   
<script>
import { required, email, sameAs, minValue } from 'vuelidate/lib/validators'
export default {
    name: 'RegistrForm',
    data() {
        return {
            form: {
                public: true,
                username: null,
                role: "Должность",
                email: null,
                password: null,
                passwordRepeat: null,
                privacy: true,
            },

            isComplete: false,
        }
    },

    validations: {
        form: {
            username: {
                required
            },
            email: {
                required,
                email
            },
            role: {
                required,
                minValue: minValue(1),                
            },
            password: {
                required
            },
            passwordRepeat: {
                required,
                sameAsPassword: sameAs('password')
            },

        },
        validationGroup: ['form.username', 'form.email', 'form.password', 'form.passwordRepeat', 'form.role']
    },
    methods: {
        sendForm: function (e) {
            let postData = {
                public: this.form.public,
                username: this.form.username,
                role: this.form.role,
                email: this.form.email,
                password: this.form.password,
                passwordRepeat: this.form.passwordRepeat,
            }

            console.log(postData);

            // Mock POST 
            fetch('https://api-endpoint.com/endpoint', {
                method: 'POST',
                headers: {
                    "Content-Type": "application/json",
                },
                body: JSON.stringify(postData),
            })
                .then((response) => {
                    if (!response.ok) {
                        throw new Error('Network response was not ok');
                    }
                    return response.json();
                })
                .then((data) => {
                    // Обработка ответа
                    console.log('Mock POST request successful:', data);
                })
                .catch((error) => {
                    // Обработка ошибки
                    console.error('Error sending mock POST request:', error);
                });

            this.isComplete = true;
            e.preventDefault();
        }
    }
}
</script>

<style>
.form-wrapper {
    margin: 0 auto;
}

.title {
    font-weight: 700;
    font-size: 19px;
    line-height: 27px;
    color: #000000;
}

.form-group {
    box-sizing: border-box;
    padding-top: 17px;
    padding-left: 31px;
    padding-right: 15px;
    border-radius: 15px;
    background-color: white;
    margin: 0 auto;
    min-width: 960px;
    min-height: 547px;
}

.form-group.hide {
    display: none;
}

.input-group {
    display: flex;
    flex-direction: column;
}

.input-group-line {
    margin: 30px 0;
    display: flex;
    justify-content: space-between;
}

.input-group-select {
    display: flex;
    flex-direction: row-reverse;
}

.form-control {
    width: 450px;
}

.form-control.error {
    border-color: red;
}

.error-message {
    color: red;
}

.form-select {
    width: 450px;
}

.form-select.error {
    border-color: red;
}

.input-group-title {
    font-weight: 500;
    font-size: 16px;
    line-height: 19px;
    color: #000000;
}

.switch-label {
    margin-bottom: 5px;
    font-weight: 500;
    font-size: 16px;
    line-height: 19px;
}

.switch-label-small {
    font-weight: 400;
    font-size: 14px;
    line-height: 19px;
    color: #696977;
}

.button-group {
    display: flex;
    justify-content: space-between;
}

.check-wrapper {
    width: 535px;
}

.btn.btn-primary.submit-button {
    width: 302px;
    height: 40px;
    background-color: #497ADA33;
    border: none;
    color: #497ADA;
    font-weight: 400;
    font-size: 12px;
    line-height: 19px;
}


.btn.btn-primary.submit-button:hover {
    color: #fff;
    background-color: #0b5ed7;
}

.btn.btn-primary.submit-button.disabled {
    background-color: #f4eaea;
}


.checkbox-label {
    font-weight: 400;
    font-size: 14px;
    line-height: 19px;
}

.checkbox-label-link {
    text-decoration: none;
    color: #3586FF;
}

.complete-group {
    display: none;
    box-sizing: border-box;
    padding: 30px;
    border-radius: 15px;
    background-color: #d8e4f4;
    margin: 0 auto;
}

.complete-group.visible {
    display: block;
}
</style>