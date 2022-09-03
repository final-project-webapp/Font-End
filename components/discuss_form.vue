<template>
    <div class="pt-20 flex">
        <b-container style="max-width: 1000px;">
            <b-card border-variant="primary" bg-variant="dark">
                <b-form @submit.prevent="onSubmit" @reset="onReset">
                    <b-form-group id="input_group_1" label="Article Title:" label-for="input_1">
                        <b-form-textarea id="input_1" v-model="form.title" type="text" placeholder=""
                            :state="validateState('title')" aria-describedby="feedback_1">
                        </b-form-textarea>
                        <b-form-invalid-feedback id="feedback_1">This is a required field.</b-form-invalid-feedback>
                    </b-form-group>

                    <b-form-group id="input_group_2" label="Writer Name:" label-for="input_2">
                        <b-form-input id="input_2" v-model="form.wname" type="text" placeholder=""
                            :state="validateState('wname')" aria-describedby="feedback_2">
                        </b-form-input>
                        <b-form-invalid-feedback id="feedback_2">This is a required field and must be at
                            least 3 characters.</b-form-invalid-feedback>
                    </b-form-group>

                    <b-form-group id="input_group_3" label="Writing Date:" label-for="input_3">
                        <b-form-input id="input_3" v-model="form.wdate" type="date" placeholder=""
                            :state="validateState('wdate')" aria-describedby="feedback_3">
                        </b-form-input>
                        <b-form-invalid-feedback id="feedback_3">This is a required field.</b-form-invalid-feedback>
                    </b-form-group>

                    <b-form-group id="input_group_4" label="Movie Name:" label-for="input_4">
                        <b-form-input id="input_4" v-model="form.mname" type="text" placeholder=""
                            :state="validateState('mname')" aria-describedby="feedback_4">
                        </b-form-input>
                        <b-form-invalid-feedback id="feedback_4">This is a required field.</b-form-invalid-feedback>
                    </b-form-group>

                    <b-form-group id="input_group_5" label="Language:">
                        <b-form-checkbox-group id="checkbox_group_1" v-model="form.lang" :state="validateState('lang')"
                            aria-describedby="feedback_5">
                            <b-form-checkbox value="TH">TH</b-form-checkbox>
                            <b-form-checkbox value="EN">EN</b-form-checkbox>
                        </b-form-checkbox-group>
                        <b-form-invalid-feedback id="feedback_5">This is a required field.</b-form-invalid-feedback>
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
import { required, minLength, maxLength } from "vuelidate/lib/validators";

export default {

    name: 'DiscussForm',
    components: {

    },
    mixins: [validationMixin],
    data() {
        return {
            form: {
                title: '',
                wname: '',
                wdate: '',
                mname: '',
                lang: [],
                
            },
            defaultview: 1,
            testuser: 1
        };
    },
    validations: {
        form: {
            title: {
                required,
                maxLength: maxLength(1000)
            },
            wname: {
                required,
                minLength: minLength(3),
                maxLength: maxLength(20)
            },
            wdate: {
                required,
            },
            mname: {
                required,
            },
            lang: {
                required
            },
        }
    },
    methods: {
        validateState(wname) {
            const { $dirty, $error } = this.$v.form[wname];
            return $dirty ? !$error : null;
        },
        onSubmit() {
            // event.preventDefault()

            this.$v.form.$touch();
            if (this.$v.form.$anyError) {
                return;
            }
            
            const discussData = {
                articles: this.form.title,
                writer: this.form.wname,
                date: this.form.wdate,
                movieName: this.form.mname,
                language: this.form.lang,
                view: this.defaultview,
                userID: this.testuser
            }
            this.$emit('discuss-data', discussData)
            console.log('data:')
            console.log(discussData)
            console.log('Form:')
            console.log(this.form.title, this.form.wname, this.form.wdate, this.form.mname, this.form.lang, this.defaultview)
            
            
            alert(JSON.stringify(this.form))

            this.form.title = ''
            this.form.wname = ''
            this.form.wdate = ''
            this.form.mname = ''
            this.form.lang = []
            this.$nextTick(() => {
                this.$v.$reset();
            });

        },

        onReset(event) {
            event.preventDefault()
            // Reset our form values
            this.form.title = ''
            this.form.wname = ''
            this.form.wdate = ''
            this.form.mname = ''
            this.form.lang = []
            // Trick to reset/clear native browser form validation state
            this.$nextTick(() => {
                this.$v.$reset();
            });
        }
    }
}
</script>