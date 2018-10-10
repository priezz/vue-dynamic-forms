<script>
import Vue from 'vue'
import {Component} from 'vue-property-decorator'

//         schema: {
//             fields: [{
//                 type: "input",
// 								inputType: "text",
//                 label: "ID",
//                 model: "id",
//                 readonly: true,
//                 featured: false,
//                 disabled: true
//             }, {
//                 type: "input",
// 								inputType: "text",
//                 label: "Name",
//                 model: "name",
//                 readonly: false,
//                 featured: true,
//                 required: true,
//                 disabled: false,
//                 placeholder: "User's name",
//                 validator: VueFormGenerator.validators.string
//             }, {
//                 type: "input",
// 								inputType: "password",
//                 label: "Password",
//                 model: "password",
//                 min: 6,
//                 required: true,
//                 hint: "Minimum 6 characters",
//                 validator: VueFormGenerator.validators.string
//             }, {
//                 type: "input",
//                 inputType: "number",
//                 label: "Age",
//                 model: "age",
// 								min: 18,
//                 validator: VueFormGenerator.validators.number
//             }, {
//                 type: "input",
// 								inputType: "email",
//                 label: "E-mail",
//                 model: "email",
//                 placeholder: "User's e-mail address",
//                 validator: VueFormGenerator.validators.email
//             }, {
//                type: "switch",
//                 label: "Status",
//                 model: "status",
//                 multi: true,
//                 readonly: false,
//                 featured: false,
//                 disabled: false,
//                 default: true,
// 								textOn: "Active",
// 								textOff: "Inactive"
//             }],
//         },



export default @Component class App extends Vue {
    newItem = ''
    model = {
        skills: [],
    }
    firstSchema = {
        fields: [{
            label: 'Какие технологии Вы используете?',
            listBox: true,
            model: 'skills',
            required: true,
            type: 'checklist',
            values: ['HTML5', 'Javascript', 'CSS3', 'CoffeeScript', 'AngularJS', 'ReactJS', 'VueJS']
        }],
    }
    formOptions = {
        validateAfterLoad: true,
        validateAfterChanged: true
    }
    title = `Номер телефона: ${this.$attrs.phone}`

    render() {
        return <div class="form">
            <h1>{this.title}</h1>
            <form-wizard 
                backButtonText="Назад"
                color="skyblue" 
                nextButtonText="Далее" 
                on-validate={this.validate}
                on-complete={this.complete}
                on-change={this.complete}
                shape="circle"
                stepSize="sm"
                subtitle=""
                title=""
            >
                <tab-content 
                    title="Выбор технологий" 
                    // before-change={this.validate}
                    // on-validate={this.validate}
                >
                    <vue-form-generator schema={this.firstSchema} model={this.model} options={this.formOptions} />
                    <form onSubmit={this.addField}>
                        <input v-model={this.newItem} />
                        <button type="submit">+</button>  
                    </form>
                </tab-content>
                
                {/** Используем для последующих шагов первые два навыка */}
                {this.model.skills.slice(0, 2).map(skill => <tab-content 
                    title={`Вопросы по ${skill}`}
                    on-validate={() => this.save()}
                >
                    <vue-form-generator schema={this.firstSchema} model={this.model} options={this.formOptions} />
                </tab-content>)}
            </form-wizard>
        </div>
    }
    addField(event) {
        // alert(`${JSON.stringify(Object.keys(this.$options))} ${JSON.stringify(Object.keys(this.$attrs))}`)
        event.preventDefault()
        if(!this.newItem) return

        this.firstSchema.fields[0].values.push(this.newItem)
        this.model.skills.push(this.newItem)
        this.newItem = ''
    }
    complete() {
        alert('hello 2')
        return true
    }
    validate() {
        alert('hello')
        return true
    }
}

</script>


<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1 {
    font-weight: normal;
    text-transform: uppercase;
}
</style>
