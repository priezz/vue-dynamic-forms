<template>
    <div class="form">
        <h1>{{title}}</h1>
        <form-wizard
            backButtonText="Назад"
            color="skyblue" 
            error-color="#e74c3c"
            @on-complete="complete"
            @on-validate="completeStep"
            nextButtonText="Далее" 
            shape="circle"
            stepSize="sm"
            subtitle=""
            ref="steps"
            title=""
        >
            <tab-content 
                :before-change="validateFirst"
                icon="ti-user"
                title="Выбор технологий" 
            >
                <vue-form-generator 
                    :schema="firstSchema" 
                    :model="formData" 
                    :options="formOptions" 
                    ref="firstForm" 
                />
                <div v-if="errorMessage">
                    <span class="error">{{errorMessage}}</span>
                </div>
                <form class="extraFields" @submit.prevent="addField">
                    <input v-model="newItem" />
                    <button type="submit">+</button>  
                </form>
            </tab-content>
            
            <!-- Используем для последующих шагов первые два навыка -->
            <tab-content 
                :before-change="validateSkill"
                v-for="skill in skills"
                :key="skill"
                :title="'Вопросы по ' + skill"
            >
                {{skill}}
                <vue-form-generator 
                    :schema="skillSchema" 
                    :model="formData.details[skill]" 
                    :options="formOptions" 
                    :ref="skill + 'Form'" 
                />
            </tab-content>
        </form-wizard>
    </div>
</template>


<script>
import Vue from 'vue'
import {validators} from 'vue-form-generator'
import {Component} from 'vue-property-decorator'


export default @Component class App extends Vue {
    errorMessage = null
    formData = {
        details: {},
        skills: [],
    }
    firstSchema = {
        fields: [{
            type: "input",
            inputType: "number",
            label: "Ваш возраст?",
            model: "age",
            max: 99,
            min: 18,
            required: true,
            validator: validators.number,
        }, {
            type: "input",
            inputType: "email",
            label: "Есть ли у Вас адрес e-mail?",
            model: "email",
            placeholder: "e-mail",
            validator: validators.email,
        }, {
            label: 'Какие технологии Вы используете?',
            listBox: true,
            model: 'skills',
            required: true,
            type: 'checklist',
            values: ['HTML5', 'Javascript', 'ReactJS', 'VueJS'],
        }],
    }
    formOptions = {
        validateAfterLoad: true,
        validateAfterChanged: true,
    }
    newItem = ''
    skillSchema = {
        fields: [{
            type: "input",
            inputType: "number",
            label: "Сколько лет используете?",
            model: "age",
            required: true,
            validator: validators.number,
        }, {
            type: "switch",
            label: "Удовлетворены ли Вы данной технологией?",
            model: "happy",
            required: true,
            textOn: "да",
            textOff: "нет",
        }, {
            type: "input",
            inputType: "text",
            label: "Комментарий",
            model: "comment",
            featured: true,
            validator: validators.string,
        }],
    }
    title = `Номер телефона: ${this.$attrs.phone}`

    constructor() {
        super()
        this.firstSchema.fields.find(f => f.model === 'skills').values.forEach(skill => {
            this.formData.details[skill] = {}
        })
    }

    get skills() {return this.formData.skills.slice(0, 2)}

    addField(event) {
        const skill = this.newItem
        if(!skill) return
        this.newItem = ''

        this.formData.details[skill] = {}
        this.firstSchema.fields.find(f => f.model === 'skills').values.push(skill)
        this.formData.skills.push(skill)
    }

    complete() {
        alert('complete')
        /** 
            Отметить в базе данных запись как завершенную
        */
    }

    validateFirst() {
        if(this.skills.length) this.errorMessage = null
        else this.errorMessage = 'Выберите хотя бы один навык'
        return this.skills.length > 0 && this.$refs.firstForm.validate()
    }

    validateSkill() {
        const skill = this.formData.skills[this.$refs.steps.activeTabIndex - 1]
        return this.$refs[`${skill}Form`][0].validate()
    }

    completeStep(isValid, tabIndex) {
        if(!isValid) return

        const skill = this.formData.skills[tabIndex - 1]
        if(tabIndex > 0) alert(skill)

        /** 
            Тут можно по необходимости сохранить промежуточный результат в базу
        */
    }
}

</script>


<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1 {
    font-size: 1.5rem;
    text-transform: uppercase;
    font-weight: normal;
}
span.error{
    color:#e74c3c;
    font-size:20px;
    display:flex;
    justify-content:center;
}
.form .extraFields {
    display: flex;
    flex-direction: 'row';
    justify-content: 'flex-start';
    width: 100%;
}
</style>
