<template> 
    <div class="form-wrapper">
    <form class='form-group'  v-bind:class="{hide: isComplete}"
            id="app"
            @submit="sendForm"
            action="api-endpoint.com/endpoint"
            method="post"
    >
        <h1  class="title">Регистрация</h1>
        <hr>
        <div class ='input-group'>
            <h3 class="input-group-title">Заполните Ваши данные</h3>
            <div class='input-group-wrapper'>
                <div class='input-group-line'>
                    <div>
                        <input class="form-control" v-bind:class="{error: errorName}" type="text"  id="name-input" placeholder="Имя" v-model="username" v-on:change="usernameChange">
                        <p class="error-message" v-if="errorName">Введите имя</p>
                    </div>
                    <div>
                        <input class="form-control" v-bind:class="{error: errorEmail}" type="email" id="email-input" placeholder="Email" v-model="email" v-on:change="emailChange">
                        <p class="error-message" v-if="errorEmail">Введите email</p>
                    </div>
                </div>
                <div class='input-group-select'>
                    <div>
                        <select class="form-select" v-bind:class="{error: errorRole}" v-model="role"  v-on:change="roleChange">
                            <option selected>Должность</option>
                            <option value="1">Frontend developer</option>
                            <option value="2">Backend developer</option>
                            <option value="3">FullStack developer</option>
                        </select>
                        <p class="error-message" v-if="errorRole">Выбеирете одну из должностей</p>
                    </div>
                </div>
                <div class='input-group-line'>
                    <div>
                        <input class="form-control" v-bind:class="{error: errorPassword}" type="password" id="password-input" placeholder="Пароль" v-model="password"  v-on:change="passwordChange">
                        <p class="error-message" v-if="errorPassword">Введите пароль</p>
                    </div>
                    <div>
                        <input class="form-control" v-bind:class="{error: errorPassword_repeat || incorrect_password}" type="password" id="password-input-1" placeholder="Повторите пароль" v-model="password_repeat" v-on:change="password_repeatChange">
                        <p class="error-message" v-if="errorPassword_repeat">Повторите пароль</p>
                        <p class="error-message" v-if="incorrect_password">Введенные пароли не совпадают</p>
                    </div>
                </div>
            </div>
        </div>
        <hr>
        <div class="form-check form-switch">
            <input class="form-check-input" type="checkbox" id="switch" checked v-model="public_akk">
            <label class="form-check-label" for="switch">
                <p class="switch-label">Хотите чтобы Ваш профиль видели другие участники платформы?</p>
                <p class="switch-label-small"> Включает профиль для просмотра другими пользователями по ссылке</p>            
            </label>
        </div>
        <div class="button-group">
            <div class="form-check check-wrapper">
                <input class="form-check-input" type="checkbox" id="checkbox" checked v-model="privacy" v-on:change="checkboxChange">
                <label class="form-check-label" for="checkbox">
                    <p class="checkbox-label">Регистрируясь, Вы соглашаетесь  с <a class="checkbox-label-link" href="#">политикой конфиденциальности</a> и обработкой <a class="checkbox-label-link" href="#">персональных данных</a></p>
                </label>
            </div>
            <button class="btn btn-primary submit-button" v-bind:class="{disabled: button_disabled}" type="submit">Зарегистрироваться</button>

        </div>
    </form>
    <div class='complete-group'  v-bind:class="{visible: isComplete}">
        <p>Регистрация успешно завершена</p>
    </div>
    </div>
</template>

<script>
    export default {
        name: 'RegistrForm',
        data(){
            return {
                public_akk: true,
                username:null,
                role: 'Должность',
                email:null,
                password:null,
                password_repeat:null,
                privacy: true,

                errorName: false,
                errorEmail: false,
                errorRole: false,
                errorPassword: false,
                errorPassword_repeat: false,
                incorrect_password: false,

                button_disabled: false,

                isComplete: false,
            } 
        },
        methods: {
            isButtonDisabled: function() {
                return (!this.privacy || this.errorName || this.errorEmail || this.errorRole ||  this.errorPassword || this.errorPassword_repeat || this.incorrect_password) ? true : false;
            },

            checkboxChange: function() {
                this.button_disabled = this.privacy ? false : true;
            },

            usernameChange: function() {
                this.errorName = this.username ? false : true;
                this.button_disabled  = this.isButtonDisabled();
            },

            emailChange: function() {
                this.errorEmail = this.email ? false : true;
                this.button_disabled  = this.isButtonDisabled();
            },

            roleChange: function() {
                this.errorRole = (this.role !== "Должность") ? false : true;
                this.button_disabled  = this.isButtonDisabled();
            },

            passwordChange: function() {
                this.errorPassword = this.password ? false : true;
                this.button_disabled  = this.isButtonDisabled();
            },

            password_repeatChange: function() {
                this.errorPassword_repeat = this.password_repeat ? false : true;
                this.incorrect_password = (this.password === this.password_repeat) ? false : true;
                this.button_disabled  = this.isButtonDisabled();
            },
            
            sendForm: function (e) {

                this.errorName = this.username ? false : true;
                this.errorEmail = this.email ? false : true;
                this.errorRole = (this.role !== "Должность") ? false : true;
                this.errorPassword = this.password ? false: true;
                this.errorPassword_repeat = this.password_repeat ? false : true;
                this.incorrect_password = (this.password === this.password_repeat) ? false : true;

                this.button_disabled  = this.isButtonDisabled();

                if (this.privacy && this.username && this.email && !this.errorRole && !this.errorPassword && !this.incorrect_password) {
                    let postData = {
                        public: this.public_akk,
                        username: this.username,
                        role: this.role,
                        email: this.email,
                        password: this.password,
                        password_repeat: this.password_repeat,
                    }
                 
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
                }
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
    font-size:16px;
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