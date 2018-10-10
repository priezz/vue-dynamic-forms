<template>
    <div id="app">
        <MyForm :phone="phone"/>
    </div>
</template>


<script>
import MyForm from "./components/MyForm";

export default {
    data: () => {
        /**
            Тут надо взять следующий телефон из базы, учитывая возможность звонка 
            в данное время (определяется по городу), а также другие ограничения 
            (кол-во перезвонов и т.п.). Для полученного номера сразу проставляем 
            в базе статус locked, чтобы для прочих операторов он игнорировался.
            Также стоит проставлять номер оператора, на котором номер был 
            заблогирован (для аналитики), а также время блокировки (для разблокировки 
            по таймауту). Полученный номер передаем в качестве параметра в MyForm.
        */
        return {phone: '+7 123 456 7890'}
    },
    name: 'App',
    components: {
        MyForm,
    },
}
</script>


<style>
#app {
    font-family: "Avenir", Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    margin-top: 60px;
}
</style>
