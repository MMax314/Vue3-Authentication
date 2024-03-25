**Vue 3 Authentication with .NET 6.0 (ASP.NET Core) JWT API**

- [Источник](#источник)
- [Комментарии](#комментарии)
- [Проект в Visual Studio Community](#проект-в-visual-studio-community)
- [Краткая характеристика функционала файлов, представленных на странице](#краткая-характеристика-функционала-файлов-представленных-на-странице)
- [Каталог .\\src\\helpers\\](#каталог-srchelpers)
  - [**.\\src\\helpers\\index.js**](#srchelpersindexjs)
  - [**.\\src\\helpers\\fake-backend.js**](#srchelpersfake-backendjs)
  - [**.\\src\\helpers\\fetch-wrapper.js**](#srchelpersfetch-wrapperjs)
  - [**.\\src\\helpers\\router.js**](#srchelpersrouterjs)
- [Каталог: .\\src\\stores](#каталог-srcstores)
  - [**.\\src\\stores\\index.js**](#srcstoresindexjs)
  - [**.\\src\\stores\\users.store.js**](#srcstoresusersstorejs)
  - [**.\\src\\stores\\auth.store.js**](#srcstoresauthstorejs)
- [Каталог: .\\src\\views](#каталог-srcviews)
  - [**.\\src\\views\\index.js**](#srcviewsindexjs)
  - [**.\\src\\views\\LoginView.vue**](#srcviewsloginviewvue)
  - [**.\\src\\views\\HomeView.vue**](#srcviewshomeviewvue)
- [Каталог: .\\src](#каталог-src)
  - [**.\\src\\App.vue**](#srcappvue)
  - [**.\\src\\main.js**](#srcmainjs)
- [Каталог .\\](#каталог-)
  - [**.\\index.html**](#indexhtml)

# Источник
* https://jasonwatmore.com/vue-3-authentication-with-net-6-aspnet-core-jwt-api#download-vue
* https://github.com/cornflourblue/vue-3-pinia-jwt-authentication-example
* [Complete documentation is available at Vue 3 + Pinia - JWT Authentication Tutorial & Example](https://jasonwatmore.com/post/2022/05/26/vue-3-pinia-jwt-authentication-tutorial-example)

# Комментарии
Комментарии созданы **Copilot (preview)** 25.03.2024

# Проект в Visual Studio Community
* Шаблон проекта: "Vue and ASP.NET Core"
* Необходимые пакеты:
  * npm install vue-router
  * npm install pinia
  * npm install vee-validate
  * npm install yup

# Краткая характеристика функционала файлов, представленных на странице
- **main.js**: Главный файл JavaScript, который инициализирует Vue-приложение, подключает маршрутизатор и хранилище состояний.
- **router.js**: Файл содержит настройки маршрутизатора для Vue-приложения, определяя маршруты и перенаправления в зависимости от статуса аутентификации пользователя.
- **fake-backend.js**: Этот файл имитирует бэкенд сервер с использованием JavaScript. Он перехватывает HTTP-запросы и возвращает фиктивные ответы, имитируя работу реального API. В нем определены функции для аутентификации пользователей и получения списка пользователей.
- **fetch-wrapper.js**: Этот файл предоставляет обертку для стандартной функции `fetch` для упрощения отправки HTTP-запросов. Он добавляет необходимые заголовки авторизации и обрабатывает ответы от API.
- **auth.store.js**: Этот файл использует библиотеку Pinia для создания хранилища состояний, которое управляет процессом аутентификации пользователя, включая вход в систему и выход из нее.
- **users.store.js**: Здесь определено хранилище для управления данными пользователей, включая загрузку списка пользователей с сервера.
- **HomeView.vue и LoginView.vue**: Эти файлы являются компонентами Vue, которые представляют домашнюю страницу и страницу входа соответственно.

# Каталог .\src\helpers\
##  **.\src\helpers\index.js**
```javascript
// Файл: .\src\helpers\index.js
// Этот файл является точкой входа для всех вспомогательных функций (helpers) проекта Vue.js.
// С помощью оператора export мы экспортируем все экспортируемые члены из других модулей.

// Экспорт всех экспортируемых членов из модуля fake-backend.
// Модуль fake-backend обычно используется для имитации запросов к серверу во время разработки.
export * from './fake-backend';

// Экспорт всех экспортируемых членов из модуля fetch-wrapper.
// Модуль fetch-wrapper предоставляет обертку для нативного API fetch, позволяя упростить и стандартизировать запросы к серверу.
export * from './fetch-wrapper';

// Экспорт всех экспортируемых членов из модуля router.
// Модуль router содержит настройки маршрутизатора Vue Router, который управляет навигацией и переходами между компонентами в приложении.
export * from './router';
```

## **.\src\helpers\fake-backend.js** 
```javascript
// Файл: .\src\helpers\fake-backend.js
// Этот файл реализует поддельный бэкенд (fake backend) для имитации работы с сервером.

// Экспорт функции fakeBackend для использования в других частях приложения.
export { fakeBackend };

// Определение функции fakeBackend.
function fakeBackend() {
    // Создание массива пользователей для имитации базы данных.
    let users = [{ id: 1, username: 'test', password: 'test', firstName: 'Test', lastName: 'User' }];
    
    // Сохранение оригинальной функции fetch в переменную.
    let realFetch = window.fetch;
    
    // Переопределение функции fetch глобального объекта window.
    window.fetch = function (url, opts) {
        // Возвращение нового промиса для асинхронной обработки.
        return new Promise((resolve, reject) => {
            // Имитация задержки вызова API сервера.
            setTimeout(handleRoute, 500);

            // Функция для обработки маршрутов API.
            function handleRoute() {
                // Определение маршрутов в зависимости от URL и метода запроса.
                switch (true) {
                    // Маршрут для аутентификации пользователя.
                    case url.endsWith('/users/authenticate') && opts.method === 'POST':
                        return authenticate();
                    // Маршрут для получения списка пользователей.
                    case url.endsWith('/users') && opts.method === 'GET':
                        return getUsers();
                    // Передача всех необработанных запросов оригинальной функции fetch.
                    default:
                        return realFetch(url, opts)
                            .then(response => resolve(response))
                            .catch(error => reject(error));
                }
            }

            //===========================================
            // Функции для обработки конкретных маршрутов:
            //===========================================

            // Функция для аутентификации пользователя.
            function authenticate() {
                // Получение данных пользователя из тела запроса.
                const { username, password } = body();
                // Поиск пользователя в массиве.
                const user = users.find(x => x.username === username && x.password === password);

                // Если пользователь не найден, возвращение ошибки.
                if (!user) return error('Username or password is incorrect');

                // Если пользователь найден, возвращение данных пользователя и токена.
                return ok({
                    id: user.id,
                    username: user.username,
                    firstName: user.firstName,
                    lastName: user.lastName,
                    token: 'fake-jwt-token'
                });
            }

            // Функция для получения списка пользователей.
            function getUsers() {
                // Проверка аутентификации пользователя.
                if (!isAuthenticated()) return unauthorized();
                // Возвращение списка пользователей.
                return ok(users);
            }

            //=========================
            // Вспомогательные функции:
            //=========================

            // Функция для формирования успешного ответа.
            function ok(body) {
                resolve({ ok: true, text: () => Promise.resolve(JSON.stringify(body)) })
            }

            // Функция для формирования ответа с ошибкой авторизации.
            function unauthorized() {
                resolve({ status: 401, text: () => Promise.resolve(JSON.stringify({ message: 'Unauthorized' })) })
            }

            // Функция для формирования ответа с ошибкой.
            function error(message) {
                resolve({ status: 400, text: () => Promise.resolve(JSON.stringify({ message })) })
            }

            // Функция для проверки аутентификации пользователя.
            function isAuthenticated() {
                // Проверка наличия токена в заголовках запроса.
                return opts.headers['Authorization'] === 'Bearer fake-jwt-token';
            }

            // Функция для получения тела запроса.
            function body() {
                // Парсинг JSON, если тело запроса существует.
                return opts.body && JSON.parse(opts.body);
            }
        });
    }
}
```

## **.\src\helpers\fetch-wrapper.js**
```javascript
// Файл: .\src\helpers\fetch-wrapper.js
// Этот файл содержит обертку для стандартной функции fetch, которая добавляет дополнительную логику, такую как автоматическое добавление заголовков аутентификации.

// Импорт хранилища аутентификации из модуля 'stores'.
import { useAuthStore } from '@/stores';

// Экспорт объекта fetchWrapper с методами для различных HTTP-запросов.
export const fetchWrapper = {
    get: request('GET'),    // Метод для GET-запросов.
    post: request('POST'),  // Метод для POST-запросов.
    put: request('PUT'),    // Метод для PUT-запросов.
    delete: request('DELETE') // Метод для DELETE-запросов.
};

// Функция для создания запросов с заданным HTTP-методом.
function request(method) {
    // Возвращает функцию, которая принимает URL и тело запроса.
    return (url, body) => {
        // Создание объекта с параметрами запроса.
        const requestOptions = {
            method, // HTTP-метод.
            headers: authHeader(url) // Заголовки запроса.
        };
        // Если есть тело запроса, добавление его в параметры запроса.
        if (body) {
            requestOptions.headers['Content-Type'] = 'application/json'; // Установка типа содержимого.
            requestOptions.body = JSON.stringify(body); // Преобразование тела запроса в строку JSON.
        }
        // Выполнение запроса с заданными параметрами и обработка ответа.
        return fetch(url, requestOptions).then(handleResponse);
    }
}
//========================
// Вспомогательные функции.
//========================
// Функция для создания заголовков аутентификации.
function authHeader(url) {
    // Получение данных пользователя из хранилища аутентификации.
    const { user } = useAuthStore();
    // Проверка, залогинен ли пользователь.
    const isLoggedIn = !!user?.token;
    // Проверка, является ли URL адресом API.
    const isApiUrl = url.startsWith(import.meta.env.VITE_API_URL);
    // Если пользователь залогинен и запрос идет на API, возвращение заголовка с токеном.
    if (isLoggedIn && isApiUrl) {
        return { Authorization: `Bearer ${user.token}` };
    } else {
        // Иначе возвращение пустого объекта заголовков.
        return {};
    }
}

// Функция для обработки ответа от сервера.
function handleResponse(response) {
    // Получение текста ответа.
    return response.text().then(text => {
        // Попытка парсинга текста ответа как JSON.
        const data = text && JSON.parse(text);
        
        // Если ответ не успешен (статус не OK).
        if (!response.ok) {
            // Получение данных пользователя и функции выхода из хранилища аутентификации.
            const { user, logout } = useAuthStore();
            // Если статус ответа 401 или 403 и пользователь залогинен, выполнение выхода.
            if ([401, 403].includes(response.status) && user) {
                logout();
            }

            // Получение сообщения об ошибке из данных ответа или статуса ответа.
            const error = (data && data.message) || response.statusText;
            // Возвращение отклоненного промиса с сообщением об ошибке.
            return Promise.reject(error);
        }

        // Если ответ успешен, возвращение данных.
        return data;
    });
}   
```

## **.\src\helpers\router.js**
```javascript
// Файл: .\src\helpers\router.js
// Этот файл отвечает за настройку маршрутизации в приложении Vue.

// Импорт функций createRouter и createWebHistory из пакета vue-router.
import { createRouter, createWebHistory } from 'vue-router';

// Импорт хука useAuthStore из каталога stores.
import { useAuthStore } from '@/stores';
// Импорт компонентов HomeView и LoginView из каталога views.
import { HomeView, LoginView } from '@/views';

// Экспорт константы router, которая создается с помощью функции createRouter.
export const router = createRouter({
    // Использование функции createWebHistory для включения истории браузера в маршрутизатор.
    history: createWebHistory(import.meta.env.BASE_URL),
    // Установка класса 'active' для активных ссылок.
    linkActiveClass: 'active',
    // Определение маршрутов приложения.
    routes: [
        // Маршрут для корневого пути, который отображает компонент HomeView.
        { path: '/', component: HomeView },
        // Маршрут для пути '/login', который отображает компонент LoginView.
        { path: '/login', component: LoginView }
    ]
});

// Глобальный хук beforeEach, который вызывается перед каждым переходом маршрута.
router.beforeEach(async (to) => {
    // Определение публичных страниц, доступ к которым не требует аутентификации.
    const publicPages = ['/login'];
    // Проверка, требуется ли аутентификация для доступа к текущему маршруту.
    const authRequired = !publicPages.includes(to.path);
    // Получение состояния аутентификации из хранилища.
    const auth = useAuthStore();

    // Если аутентификация требуется и пользователь не аутентифицирован,
    // сохранение запрашиваемого пути для последующего редиректа после входа в систему
    // и перенаправление на страницу входа.
    if (authRequired && !auth.user) {
        auth.returnUrl = to.fullPath;
        return '/login';
    }
});
```

# Каталог: .\src\stores
## **.\src\stores\index.js**
```javascript
// Файл: .\src\stores\index.js
// Этот файл служит центральной точкой экспорта для всех хранилищ (store modules) в проекте Vue.js.

// Экспорт всех экспортируемых членов из модуля 'auth.store'.
// Модуль 'auth.store' содержит состояние и логику, связанные с аутентификацией пользователя.
export * from './auth.store';

// Экспорт всех экспортируемых членов из модуля 'users.store'.
// Модуль 'users.store' управляет состоянием и действиями, связанными с данными пользователей.
export * from './users.store';

```
## **.\src\stores\users.store.js**
```javascript
// Файл: .\src\stores\users.store.js
// Этот файл содержит хранилище для управления данными пользователей в приложении Vue с использованием Pinia.

// Импорт функции defineStore из библиотеки Pinia, которая используется для создания хранилища.
import { defineStore } from 'pinia';

// Импорт объекта fetchWrapper из каталога helpers, который предоставляет методы для сетевых запросов.
import { fetchWrapper } from '@/helpers';

// Определение базового URL для API пользователей, используя переменные окружения.
const baseUrl = `${import.meta.env.VITE_API_URL}/users`;

// Экспорт функции useUsersStore, которая определяет хранилище с идентификатором 'users'.
export const useUsersStore = defineStore({
    // Уникальный идентификатор хранилища.
    id: 'users',
    // Функция state возвращает объект состояния хранилища.
    state: () => ({
        // Объект users для хранения данных о пользователях.
        users: {}
    }),
    // Объект actions содержит методы для изменения состояния хранилища.
    actions: {
        // Асинхронный метод getAll для получения данных о всех пользователях.
        async getAll() {
            // Установка свойства loading в true перед началом загрузки данных.
            this.users = { loading: true };
            // Использование fetchWrapper для отправки GET-запроса на baseUrl.
            fetchWrapper.get(baseUrl)
                // Обработка успешного ответа: сохранение полученных данных о пользователях в состояние хранилища.
                .then(users => this.users = users)
                // Обработка ошибки: сохранение объекта ошибки в состояние хранилища.
                .catch(error => this.users = { error })
        }
    }
});
```
## **.\src\stores\auth.store.js**
```javascript
// Файл: .\src\stores\auth.store.js
// Этот файл содержит хранилище для управления аутентификацией пользователя в приложении Vue с использованием Pinia.

// Импорт функции defineStore из библиотеки Pinia для создания хранилища.
import { defineStore } from 'pinia';

// Импорт объекта fetchWrapper и router из каталога helpers.
// fetchWrapper используется для выполнения сетевых запросов, а router для управления маршрутизацией.
import { fetchWrapper, router } from '@/helpers';

// Определение базового URL для API аутентификации, используя переменные окружения.
const baseUrl = `${import.meta.env.VITE_API_URL}/users`;

// Экспорт функции useAuthStore, которая определяет хранилище с идентификатором 'auth'.
export const useAuthStore = defineStore({
    // Уникальный идентификатор хранилища 'auth'.
    id: 'auth',
    // Функция state определяет начальное состояние хранилища.
    state: () => ({
        // Состояние user инициализируется из localStorage, чтобы пользователь оставался в системе между обновлениями страницы.
        user: JSON.parse(localStorage.getItem('user')),
        // returnUrl используется для сохранения URL, на который нужно перенаправить пользователя после аутентификации.
        returnUrl: null
    }),
    // Объект actions содержит методы для изменения состояния хранилища.
    actions: {
        // Асинхронный метод login для входа пользователя в систему.
        async login(username, password) {
            // Отправка запроса на аутентификацию и ожидание ответа.
            const user = await fetchWrapper.post(`${baseUrl}/authenticate`, { username, password });

            // Обновление состояния user в хранилище Pinia.
            this.user = user;

            // Сохранение данных пользователя и JWT в localStorage для поддержания сессии между обновлениями страницы.
            localStorage.setItem('user', JSON.stringify(user));

            // Перенаправление на сохраненный URL или на главную страницу, если returnUrl не задан.
            router.push(this.returnUrl || '/');
        },
        // Метод logout для выхода пользователя из системы.
        logout() {
            // Очистка состояния user.
            this.user = null;
            // Удаление данных пользователя из localStorage.
            localStorage.removeItem('user');
            // Перенаправление на страницу входа.
            router.push('/login');
        }
    }
});
```

# Каталог: .\src\views
## **.\src\views\index.js**
```javascript
// Файл: .\src\views\index.js
// Этот файл служит точкой экспорта для компонентов представлений (views) в проекте Vue.js.

// Экспорт компонента HomeView как default export из файла HomeView.vue.
// Компонент HomeView обычно отображает главную страницу приложения.
export { default as HomeView } from './HomeView.vue';

// Экспорт компонента LoginView как default export из файла LoginView.vue.
// Компонент LoginView используется для создания интерфейса страницы входа в систему.
export { default as LoginView } from './LoginView.vue';
```

## **.\src\views\LoginView.vue**
```javascript
// Файл: .\src\views\LoginView.vue
// Этот файл определяет компонент Vue для страницы входа в систему.

<script setup>
// Импорт компонентов Form и Field из библиотеки vee-validate для валидации форм.
import { Form, Field } from 'vee-validate';
// Импорт библиотеки Yup для определения схемы валидации.
import * as Yup from 'yup';

// Импорт хука useAuthStore для работы с состоянием аутентификации.
import { useAuthStore } from '@/stores';

// Определение схемы валидации для полей формы.
const schema = Yup.object().shape({
    username: Yup.string().required('Username is required'), // Поле username должно быть строкой и обязательным.
    password: Yup.string().required('Password is required') // Поле password должно быть строкой и обязательным.
});

// Функция onSubmit вызывается при отправке формы.
function onSubmit(values, { setErrors }) {
    // Получение доступа к хранилищу аутентификации.
    const authStore = useAuthStore();
    // Деструктуризация значений формы.
    const { username, password } = values;

    // Вызов метода login хранилища и обработка возможной ошибки.
    return authStore.login(username, password)
        .catch(error => setErrors({ apiError: error })); // Установка ошибки API, если она возникла.
}
</script>

<template>
    <div>
        <!-- Информационное сообщение с тестовыми данными для входа. -->
        <div class="alert alert-info">
            Username: test<br />
            Password: test
        </div>
        <!-- Заголовок формы входа. -->
        <h2>Login</h2>
        <!-- Компонент Form из vee-validate с привязкой схемы валидации и слотом для ошибок и состояния отправки. -->
        <Form @submit="onSubmit" :validation-schema="schema" v-slot="{ errors, isSubmitting }">
            <!-- Группа формы для ввода имени пользователя. -->
            <div class="form-group">
                <label>Username</label>
                <!-- Компонент Field для ввода имени пользователя с валидацией. -->
                <Field name="username" type="text" class="form-control" :class="{ 'is-invalid': errors.username }" />
                <!-- Отображение сообщения об ошибке для поля username. -->
                <div class="invalid-feedback">{{errors.username}}</div>
            </div>            
            <!-- Группа формы для ввода пароля. -->
            <div class="form-group">
                <label>Password</label>
                <!-- Компонент Field для ввода пароля с валидацией. -->
                <Field name="password" type="password" class="form-control" :class="{ 'is-invalid': errors.password }" />
                <!-- Отображение сообщения об ошибке для поля password. -->
                <div class="invalid-feedback">{{errors.password}}</div>
            </div>            
            <!-- Кнопка отправки формы. -->
            <div class="form-group">
                <button class="btn btn-primary" :disabled="isSubmitting">
                    <!-- Индикатор загрузки при отправке формы. -->
                    <span v-show="isSubmitting" class="spinner-border spinner-border-sm mr-1"></span>
                    Login
                </button>
            </div>
            <!-- Отображение ошибки API, если она есть. -->
            <div v-if="errors.apiError" class="alert alert-danger mt-3 mb-0">{{errors.apiError}}</div>
        </Form>
    </div>
</template>   
```
## **.\src\views\HomeView.vue**
```javascript
// Файл: .\src\views\HomeView.vue
// Этот файл определяет компонент Vue для домашней страницы приложения.

<script setup>
// Импорт функции storeToRefs из библиотеки Pinia для реактивного доступа к состоянию хранилища.
import { storeToRefs } from 'pinia';

// Импорт хуков useAuthStore и useUsersStore для работы с хранилищами аутентификации и пользователей.
import { useAuthStore, useUsersStore } from '@/stores';

// Создание экземпляра хранилища аутентификации и получение реактивной ссылки на пользователя.
const authStore = useAuthStore();
const { user: authUser } = storeToRefs(authStore);

// Создание экземпляра хранилища пользователей и получение реактивной ссылки на список пользователей.
const usersStore = useUsersStore();
const { users } = storeToRefs(usersStore);

// Вызов метода getAll для получения списка пользователей при инициализации компонента.
usersStore.getAll();
</script>

<template>
    <div>
        <!-- Приветствие пользователя, используя его имя, полученное из хранилища аутентификации. -->
        <h1>Hi {{authUser?.firstName}}!</h1>
        <!-- Информационное сообщение о том, что пользователь вошел в систему. -->
        <p>You're logged in with Vue 3 + Pinia & JWT!!</p>
        <!-- Заголовок для списка пользователей, полученных с защищенного API. -->
        <h3>Users from secure api end point:</h3>
        <!-- Условный рендеринг списка пользователей, если он не пустой. -->
        <ul v-if="users.length">
            <!-- Итерация по списку пользователей и отображение их имен. -->
            <li v-for="user in users" :key="user.id">{{user.firstName}} {{user.lastName}}</li>
        </ul>
        <!-- Индикатор загрузки, отображаемый во время получения данных о пользователях. -->
        <div v-if="users.loading" class="spinner-border spinner-border-sm"></div>
        <!-- Сообщение об ошибке, если при загрузке пользователей произошла ошибка. -->
        <div v-if="users.error" class="text-danger">Error loading users: {{users.error}}</div>
    </div>
</template>   
```

# Каталог: .\src
## **.\src\App.vue**
```javascript
// Файл: .\src\App.vue
// Этот файл является корневым компонентом приложения Vue и определяет общий макет приложения.

<script setup>
// Импорт компонентов RouterLink и RouterView из пакета vue-router.
import { RouterLink, RouterView } from 'vue-router';

// Импорт хука useAuthStore из каталога stores для работы с состоянием аутентификации.
import { useAuthStore } from '@/stores';

// Создание экземпляра хранилища аутентификации для доступа к его состоянию и действиям.
const authStore = useAuthStore();
</script>

<template>
    <!-- Корневой контейнер приложения с классами для стилей. -->
    <div class="app-container bg-light">
        <!-- Навигационная панель, отображаемая только если пользователь вошел в систему. -->
        <nav v-show="authStore.user" class="navbar navbar-expand navbar-dark bg-dark">
            <!-- Контейнер для элементов навигации. -->
            <div class="navbar-nav">
                <!-- Компонент RouterLink для навигации по маршруту '/', текст ссылки - 'Home'. -->
                <RouterLink to="/" class="nav-item nav-link">Home</RouterLink>
                <!-- Ссылка для выхода из системы, при клике вызывается метод logout из хранилища аутентификации. -->
                <a @click="authStore.logout()" class="nav-item nav-link">Logout</a>
            </div>
        </nav>
        <!-- Контейнер для отображения содержимого маршрута, определенного в RouterView. -->
        <div class="container pt-4 pb-4">
            <!-- Компонент RouterView отображает компонент, соответствующий текущему маршруту. -->
            <RouterView />
        </div>
    </div>
</template>

<style>
<!-- Импорт общих стилей для приложения из файла base.css. -->
@import '@/assets/base.css';
</style>
```

## **.\src\main.js**
```javascript
// Файл: .\src\main.js
// Этот файл является точкой входа в приложение Vue и отвечает за инициализацию корневого компонента и плагинов.

// Импорт функции createApp из фреймворка Vue, которая используется для создания экземпляра приложения.
import { createApp } from 'vue';
// Импорт функции createPinia из библиотеки Pinia, которая используется для создания хранилища состояния.
import { createPinia } from 'pinia';

// Импорт корневого компонента App из файла App.vue.
import App from './App.vue';
// Импорт экземпляра маршрутизатора из каталога helpers.
import { router } from './helpers';

// Настройка фиктивного бэкенда для имитации работы с сервером.
// Импорт функции fakeBackend из каталога helpers.
import { fakeBackend } from './helpers';
// Вызов функции fakeBackend для активации фиктивного бэкенда.
fakeBackend();

// Создание экземпляра приложения Vue, передавая корневой компонент App.
const app = createApp(App);

// Регистрация Pinia в приложении Vue для управления состоянием.
app.use(createPinia());
// Регистрация маршрутизатора в приложении Vue для управления навигацией.
app.use(router);

// Монтирование приложения Vue к элементу DOM с идентификатором 'app'.
app.mount('#app'); 
```

# Каталог .\
## **.\index.html**
```html
<!-- Файл: .\index.html -->
<!-- Это основной HTML-файл, который служит входной точкой для пользовательского интерфейса приложения Vue.js. -->

<!DOCTYPE html>
<!-- DOCTYPE определяет версию HTML для браузера, здесь указан HTML5. -->
<html lang="en">
<!-- Открывающий тег html с атрибутом lang, указывающим на основной язык содержимого (английский). -->

<head>
    <!-- Секция head содержит метаданные и ссылки на внешние ресурсы. -->
    <meta charset="UTF-8" />
    <!-- Мета-тег charset указывает кодировку символов документа (UTF-8). -->
    <link rel="icon" href="/favicon.ico" />
    <!-- Ссылка на иконку веб-сайта (favicon), которая отображается во вкладке браузера. -->
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <!-- Мета-тег viewport управляет масштабированием и размерами окна просмотра на мобильных устройствах. -->
    <title>Vue 3 + Pinia - JWT Authentication Example</title>
    <!-- Заголовок веб-страницы, отображаемый в заголовке вкладки браузера. -->

    <!-- bootstrap css -->
    <link href="//netdna.bootstrapcdn.com/bootstrap/4.5.2/css/bootstrap.min.css" rel="stylesheet" />
    <!-- Подключение минифицированной версии CSS-фреймворка Bootstrap для стилизации веб-страницы. -->
</head>

<body>
    <!-- Тело HTML-документа, содержащее видимое содержимое страницы. -->
    <div id="app"></div>
    <!-- Контейнер с идентификатором 'app', который будет использоваться Vue.js для монтирования приложения. -->
    <script type="module" src="/src/main.js"></script>
    <!-- Подключение JavaScript-модуля main.js, который является точкой входа в приложение Vue.js. -->

    <!-- credits -->
    <div class="text-center mt-4">
        <!-- Блок с кредитами, выровненный по центру и с отступом сверху. -->
        <p>
            <!-- Параграф с ссылкой на учебное пособие по аутентификации с использованием Vue 3 и Pinia. -->
            <a href="https://jasonwatmore.com/post/2022/05/26/vue-3-pinia-jwt-authentication-tutorial-example">Vue 3 + Pinia - JWT Authentication Example & Tutorial</a>
        </p>
        <p>
            <!-- Параграф с ссылкой на веб-сайт автора учебного пособия. -->
            <a href="https://jasonwatmore.com">JasonWatmore.com</a>
        </p>
    </div>
</body>

</html>   
```