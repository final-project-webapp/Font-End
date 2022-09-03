<template>
    <div class="pt-20 flex">
        <b-container style="max-width: 800px;">
            <b-card border-variant="primary" bg-variant="dark">
                <b-form @submit="onSubmit">
                    <b-form-group id="input_group_1" label="Name:" label-for="input_1">
                        <b-form-input id="input_name" v-model="form.name" type="text" placeholder=""
                            :state="validateState('name')" aria-describedby="feedback_name">
                        </b-form-input>
                        <b-form-invalid-feedback id="feedback_name">This is a required field and must be at
                            least 3 characters and not more than 20 characters.</b-form-invalid-feedback>
                    </b-form-group>

                    <b-form-group id="input_group_2" label="Email:" label-for="input_2">
                        <b-form-input id="input_email" v-model="form.email" type="email" placeholder=""
                            :state="validateState('email')" aria-describedby="feedback_email">
                        </b-form-input>
                        <b-form-invalid-feedback id="feedback_email">This is a required field.</b-form-invalid-feedback>
                    </b-form-group>

                    <b-form-group id="input_group_3" label="Password:" label-for="input_3">
                        <b-form-input id="input_pass" v-model="form.password" type="password" placeholder="" rows="8"
                            :state="validateState('password')" aria-describedby="feedback_pass">
                        </b-form-input>
                        <b-form-invalid-feedback id="feedback_pass">This is a required field and must be at
                            least 8 characters and not more than 24 characters..</b-form-invalid-feedback>
                    </b-form-group>

                    <b-form-group id="input_group_3" label="DOB:" label-for="input_3">
                        <b-form-input id="input_DOB" v-model="form.DOB" type="date" placeholder="" rows="8"
                            :state="validateState('date')" aria-describedby="feedback_DOB">
                        </b-form-input>
                        <b-form-invalid-feedback id="feedback_DOB">This is a required field.</b-form-invalid-feedback>
                    </b-form-group>
                    <div class="justify-center flex">
                        <b-button type="submit" variant="primary">Register</b-button>
                        <!-- <b-button type="reset" variant="danger">Reset</b-button> -->
                    </div>
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
import { required, minLength, maxLength, email } from "vuelidate/lib/validators";

export default {
    name: 'RegisterForm',
    components: {

    },
    mixins: [validationMixin],
    emits: ['register_user'],
    data() {
        return {
            form: {
                name: '',
                emailaddress: '',
                password: '',
                DOB: ''
            },
        };
    },
    validations: {
        form: {
            name: {
                required,
                minLength: minLength(3),
                maxLength: maxLength(20),
            },
            emailaddress: {
                required,
                email
            },
            password: {
                required,
                minLength: minLength(8),
                maxLength: maxLength(24)
            },
            DOB: {
                required
            },
        }
    },
    methods: {
        validateState() {
            const { $dirty, $error } = this.$v.form;
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
        // onReset(event) {
        //     event.preventDefault()
        //     // Reset our form values        
        //     this.form.name = ''
        //     this.form.emailaddress = ''
        //     this.form.password = ''
        //     this.form.tel = ''
        //     this.form.DOB = ''
        //     // Trick to reset/clear native browser form validation state
        //     this.$nextTick(() => {
        //         this.$v.$reset();
        //     });
        // }
    }
}
</script>