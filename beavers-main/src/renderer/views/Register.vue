<template>
    <div class="register">
        <div class="header">Регистрация</div>
        <form @submit.prevent="register">
            <div class="form">
                <input v-model="username" type="text" placeholder="Имя пользователя" required>
                <input v-model="email" type="text" placeholder="Почта" required>
                <input v-model="password" type="password" placeholder="Пароль" required id="password">
            </div>
            <div class="buttons">
                <PixelButton type="submit">ВОЙТИ</PixelButton>
                <router-link to="/" class="main-button">
                    <PixelButton>НАЗАД</PixelButton>
                </router-link>
            </div>
        </form>
    </div>
</template>

<script lang="ts">
import http from "../http_common";
import PixelButton from "../components/FancyButton.vue";

export default {
    components: {
        PixelButton
    },
    data() {
        return {
            username: '',
            email: '',
            password: '',
        };
    },
    methods: {
        async register() {
            try {
                const response = await http.post('/register/', {
                    username: this.username,
                    email: this.email,
                    password: this.password,
                });

                // Убираем проверку на наличие токена, чтобы сохранить username
                const token = response.data.token;
                if (token) {
                    // Сохранение username в localStorage
                    localStorage.setItem('username', this.username);
                    localStorage.setItem('token', token);

                    // Перенаправление на главную страницу
                    this.$router.push("/");
                }
            } catch (error) {
                console.error(error);
            }
        },
    },
};
</script>

<style scoped lang="css">
.register {
    display: flex;
    flex-direction: column;
    align-items: center;
}

.header {
    font-size: 30px;
    text-align: center;
    padding: 20px;
    color: #fcfcfc;
    font-weight: 700;
}

.form {
    display: flex;
    flex-direction: column;
    padding: 10px 10px 2px;
    align-items: center;
}

.form input {
    margin-bottom: 10px;
}

.buttons {
    display: flex;
    flex-direction: column;
    align-items: center;
}

input {
    padding: 5px;
    background: none;
    border: 3px solid #aa70da;
    font-size: 20px;
    font-family: 'Pixelify Sans', sans-serif;
    color: #ffffff; /* Цвет текста в полях ввода */
}

input::placeholder {
    color: #ffffff; /* Цвет текста плейсхолдера */
}

input:focus {
    outline: none;
}

.enter {
    font-size: 50px !important;
    color: #ffffff;
}

a {
    text-decoration: none;
}
</style>