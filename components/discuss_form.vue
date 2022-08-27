<template>
    <div class="pt-20 flex">
        <b-container>
            <b-card border-variant="primary" bg-variant="dark">
                <b-form @submit="onSubmit" @reset="onReset">
                    <b-form-group id="input_group_1" label="Discuss Title:" label-for="input_1">
                        <b-form-input id="input_1" v-model="form.title" type="text" placeholder="" :state="validateState('title')" aria-describedby="feedback_1">
                        </b-form-input>
                        <b-form-invalid-feedback id="feedback_1">This is a required field.</b-form-invalid-feedback>
                    </b-form-group>

                    <b-form-group id="input_group_2" label="Writer Name:" label-for="input_2">
                        <b-form-input id="input_2" v-model="form.name" type="text" placeholder="" :state="validateState('name')" aria-describedby="feedback_2">
                        </b-form-input>
                        <b-form-invalid-feedback id="feedback_2">This is a required field and must be at
                            least 3 characters.</b-form-invalid-feedback>

                    </b-form-group>

                    <b-form-group id="input_group_3" label="Discuss Article:" label-for="input_3">
                        <b-form-textarea id="input_3" v-model="form.article" placeholder="" rows="8" :state="validateState('article')" aria-describedby="feedback_3">
                        </b-form-textarea>
                        <b-form-invalid-feedback id="feedback_3">This is a required field.</b-form-invalid-feedback>
                    </b-form-group>

                    <b-form-group id="input_group_4" label="Language:">
                        <b-form-checkbox-group id="checkbox_group_1" v-model="form.lang" :state="validateState('lang')" aria-describedby="feedback_4">
                            <b-form-checkbox value="TH">TH</b-form-checkbox>
                            <b-form-checkbox value="EN">EN</b-form-checkbox>
                        </b-form-checkbox-group>
                        <b-form-invalid-feedback id="feedback_4">This is a required field.</b-form-invalid-feedback>
                    </b-form-group>

                    <b-button type="submit" variant="primary">Submit</b-button>
                    <b-button type="reset" variant="danger">Reset</b-button>
                </b-form>


            </b-card>
            <div>


                <b-card class="mt-3" header="Form Data Result">
                    <pre class="m-0">{{ form }}</pre>
                </b-card>
            </div>
        </b-container>
    </div>
</template>

<script>
import { validationMixin } from "vuelidate";
import { required, minLength } from "vuelidate/lib/validators";

export default {
    name: 'DiscussForm',
    components: {

    },
    mixins: [validationMixin],
    data() {
        return {
            form: {
                title: '',
                name: '',
                article: '',
                lang: []
            }
        };
    },
    validations: {
        form: {
            title: {
                required
            },
            name: {
                required,
                minLength: minLength(3)
            },
            article: {
                required
            },
            lang: {
                required
            },
        }
    },
    methods: {
        validateState(name) {
            const { $dirty, $error } = this.$v.form [name];
            return $dirty ? !$error : null;
        },
        onSubmit(event) {
            event.preventDefault()
           
            this.$v.form.$touch();
            if (this.$v.form.$anyError) {
                return;
            }
            alert(JSON.stringify(this.form))
            
        },
        onReset(event) {
            event.preventDefault()
            // Reset our form values
            this.form.title = ''
            this.form.name = ''
            this.form.article = ''
            this.form.lang = []
            // Trick to reset/clear native browser form validation state
            this.$nextTick(() => {
                this.$v.$reset();
            });
        }
    }
}
</script>