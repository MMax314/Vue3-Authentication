<!-- Это Vue.js компонент, который используется для отображения основного интерфейса приложения -->

<!--
Тег <script setup> используется в Vue.js 3 для обозначения нового синтаксиса Composition API в одном файле компонента (SFC). 
Этот синтаксис позволяет использовать функции Composition API прямо в теге <script>, без необходимости создавать отдельную setup() функцию.
Слово setup указывает на то, что в этом блоке <script> будет использоваться синтаксис Composition API. 
Это позволяет упростить структуру кода и сделать его более читаемым, особенно при работе с большим количеством реактивных переменных и функций.
-->
<script setup>
    // Импортируем компоненты RouterLink и RouterView из vue-router для маршрутизации
    import { RouterLink, RouterView } from 'vue-router';
    // Импортируем хук useAuthStore из '@/stores' для работы с авторизацией
    import { useAuthStore } from '@/stores';
    // Инициализируем хранилище авторизации
    const authStore = useAuthStore();
</script>

<template>
    <!-- Создаем контейнер для приложения с классами "app-container" и "bg-light" -->
    <div class="app-container bg-light">
        <!-- Создаем навигационную панель, которая отображается только если пользователь авторизован (authStore.user) -->
        <nav v-show="authStore.user" class="navbar navbar-expand navbar-dark bg-dark">
            <!-- Создаем контейнер для элементов навигации -->
            <div class="navbar-nav">
                <!-- Создаем ссылку на главную страницу -->
                <RouterLink to="/" class="nav-item nav-link">Home</RouterLink>
                <!-- Создаем ссылку для выхода из системы, при клике на которую вызывается метод logout() из хранилища авторизации -->
                <a @click="authStore.logout()" class="nav-item nav-link">Logout</a>
            </div>
        </nav>
        <!-- Создаем контейнер для основного содержимого страницы -->
        <div class="container pt-4 pb-4">
            <!-- Вставляем динамический контент, который зависит от текущего маршрута -->
            <RouterView />
        </div>
    </div>
</template>

<style>
    /* Импортируем базовые стили для приложения */
    @import '@/assets/base.css';
</style>