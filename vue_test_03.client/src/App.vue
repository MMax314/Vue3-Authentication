<!-- ��� Vue.js ���������, ������� ������������ ��� ����������� ��������� ���������� ���������� -->

<!--
��� <script setup> ������������ � Vue.js 3 ��� ����������� ������ ���������� Composition API � ����� ����� ���������� (SFC). 
���� ��������� ��������� ������������ ������� Composition API ����� � ���� <script>, ��� ������������� ��������� ��������� setup() �������.
����� setup ��������� �� ��, ��� � ���� ����� <script> ����� �������������� ��������� Composition API. 
��� ��������� ��������� ��������� ���� � ������� ��� ����� ��������, �������� ��� ������ � ������� ����������� ���������� ���������� � �������.
-->
<script setup>
    // ����������� ���������� RouterLink � RouterView �� vue-router ��� �������������
    import { RouterLink, RouterView } from 'vue-router';
    // ����������� ��� useAuthStore �� '@/stores' ��� ������ � ������������
    import { useAuthStore } from '@/stores';
    // �������������� ��������� �����������
    const authStore = useAuthStore();
</script>

<template>
    <!-- ������� ��������� ��� ���������� � �������� "app-container" � "bg-light" -->
    <div class="app-container bg-light">
        <!-- ������� ������������� ������, ������� ������������ ������ ���� ������������ ����������� (authStore.user) -->
        <nav v-show="authStore.user" class="navbar navbar-expand navbar-dark bg-dark">
            <!-- ������� ��������� ��� ��������� ��������� -->
            <div class="navbar-nav">
                <!-- ������� ������ �� ������� �������� -->
                <RouterLink to="/" class="nav-item nav-link">Home</RouterLink>
                <!-- ������� ������ ��� ������ �� �������, ��� ����� �� ������� ���������� ����� logout() �� ��������� ����������� -->
                <a @click="authStore.logout()" class="nav-item nav-link">Logout</a>
            </div>
        </nav>
        <!-- ������� ��������� ��� ��������� ����������� �������� -->
        <div class="container pt-4 pb-4">
            <!-- ��������� ������������ �������, ������� ������� �� �������� �������� -->
            <RouterView />
        </div>
    </div>
</template>

<style>
    /* ����������� ������� ����� ��� ���������� */
    @import '@/assets/base.css';
</style>