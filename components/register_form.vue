<template>
    <div class="pt-20 flex">
        <b-container style="max-width: 800px;">
            <b-card border-variant="primary" bg-variant="dark">
                <b-form @submit.prevent="onSubmit">
                    <b-form-group id="input_group_1" label="Name:" label-for="input_1">
                        <b-form-input id="input_name" v-model="form.uname" type="text" placeholder=""
                        :state="validateState('uname')" aria-describedby="feedback_1">
                        </b-form-input>
                        <b-form-invalid-feedback id="feedback_1">This is a required field and must be at
                            least 3 characters and not more than 20 characters.</b-form-invalid-feedback>
                    </b-form-group>

                    <b-form-group id="input_group_2" label="Penname:" label-for="input_2">
                        <b-form-input id="input_penname" v-model="form.pname" type="text" placeholder=""
                        :state="validateState('pname')" aria-describedby="feedback_2">
                        </b-form-input>
                        <b-form-invalid-feedback id="feedback_2">This is a required field and must be at
                            least 3 characters and not more than 20 characters.</b-form-invalid-feedback>
                    </b-form-group>

                    <b-form-group id="input_group_3" label="Email:" label-for="input_3">
                        <b-form-input id="input_email" v-model="form.emailaddress" type="email" placeholder=""
                        :state="validateState('emailaddress')" aria-describedby="feedback_3">
                        </b-form-input>
                        <b-form-invalid-feedback id="feedback_3">This is a required field.</b-form-invalid-feedback>
                    </b-form-group>

                    <b-form-group id="input_group_4" label="Password:" label-for="input_4">
                        <b-form-input id="input_pass" v-model="form.password" type="password" placeholder="" rows="8"
                        :state="validateState('password')" aria-describedby="feedback_4">
                        </b-form-input>
                        <b-form-invalid-feedback id="feedback_4">This is a required field and must be at
                            least 8 characters and not more than 24 characters.</b-form-invalid-feedback>
                    </b-form-group>

                    <b-form-group id="input_group_5" label="Birthdate:" label-for="input_5">
                        <b-form-input id="input_DOB" v-model="form.date" type="date" placeholder="" rows="8"
                        :state="validateState('date')" aria-describedby="feedback_5">
                        </b-form-input>
                        <b-form-invalid-feedback id="feedback_5">This is a required field.</b-form-invalid-feedback>
                    </b-form-group>
                    <div class="justify-center flex">
                        <b-button type="submit" variant="primary">Register</b-button>
                        <!-- <b-button type="reset" variant="danger">Reset</b-button> -->
                    </div>
                </b-form>
            </b-card>
            <div>
                <!-- <b-card class="mt-3" header="Form Data Result">
                    <pre class="m-0">{{ form }}</pre>
                </b-card> -->
            </div>
        </b-container>
    </div>
</template>
<script>
import { validationMixin } from "vuelidate";
import { required, minLength, maxLength, email } from "vuelidate/lib/validators";

export default {    
    components: {

    },
    mixins: [validationMixin],    
    data() {
        return {
            form: {
                uname: '',
                pname:'',
                emailaddress: '',
                password: '',
                date: ''
            },
        };
    },
    validations: {
        form: {
            uname: {
                required,
                minLength: minLength(3),
                maxLength: maxLength(20),
            },
            pname: {
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
            date: {
                required
            },
        }
    },
    methods: {
        validateState(uname) {
            const { $dirty, $error } = this.$v.form[uname];
            return $dirty ? !$error : null;
        },
        onSubmit() {                            
                this.$v.form.$touch();
                if (this.$v.form.$anyError) {
                    return;
                }

                const registerData = {
                    userName: this.form.uname,
                    penname: this.form.pname,
                    emailAddress: this.form.emailaddress,
                    password: this.form.password,
                    dob: this.form.date
                }
                this.$emit('register-user', registerData)
                console.log('regisdata:')
                console.log(registerData)

                this.form.uname = ''
                this.form.pname = ''
                this.form.emailaddress = ''
                this.form.password = ''
                this.form.date = ''
                this.$nextTick(() => {
                    this.$v.$reset();
                });              

        },
        // onReset(event) {
        //     event.preventDefault()
        //     // Reset our form values        
        //     this.form.uname = ''
        //     this.form.emailaddress = ''
        //     this.form.password = ''           
        //     this.form.DOB = ''
        //     // Trick to reset/clear native browser form validation state
        //     this.$nextTick(() => {
        //         this.$v.$reset();
        //     });
        // }
    }
}
</script>