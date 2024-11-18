<template>
    <div class="login-form">
        <div class="header">
            Авторизация
        </div>
        <form @submit.prevent="login">
            <div class="form">
                <input v-model="username" type="text" placeholder="Имя пользователя" required>
                <input v-model="password" type="password" placeholder="Пароль" required id="password">
            </div>
            <div class="in">
                <!--check login and password-->
                <PixelButton type="submit" class="enter">
                    ВОЙТИ
                </PixelButton>
            
                <div>
                    <router-link to="/" class="no-underline">
                        <PixelButton class="enter">
                            НАЗАД
                        </PixelButton>
                    </router-link>
                </div>
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
            password: '',
            isShow: false,
            isAuth: false,
        };
    },

    methods: {
        async login() {
          try {
            const response = await http.post('/login/', {
              username: this.username,
              password: this.password,
            });

            const token = response.data.token;

            if (token) {
              localStorage.setItem('token', token);
              localStorage.setItem('username', this.username); // Сохраняем имя пользователя
              this.isAuth = true;
              this.$router.push("/");
            }
          }
          catch (error) {
            console.error(error);
          }
        }
    }
};
</script>

<style scoped lang="css">

.in {
    display: flex;
    flex-direction: column;
    align-items: center;
}

.login-form {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
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
    cursor: pointer;
    color: #fcfcfc;
}

/* Добавленные стили */
.no-underline {
    text-decoration: none;
}

.no-underline:link, 
.no-underline:visited, 
.no-underline:hover, 
.no-underline:active {
    text-decoration: none;
    color: inherit; /* Наследовать цвет от родительского элемента */
}
</style>
