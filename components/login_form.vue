<template>
    <div class="pt-20 flex">
        <b-container style="max-width: 400px;">
            <b-card border-variant="primary" bg-variant="dark">
                <b-form @submit="onSubmit">

                    <b-form-group id="input_group_1" label="Email:" label-for="input_1">
                        <b-form-input id="input_email" v-model="form.email" type="email" placeholder=""
                            :state="validateState('emailaddress')" aria-describedby="feedback_1">
                        </b-form-input>
                        <b-form-invalid-feedback id="feedback_1">This is a required field and must be at
                            least 3 characters.</b-form-invalid-feedback>
                    </b-form-group>

                    <b-form-group id="input_group_2" label="Password:" label-for="input_2">
                        <b-form-input id="input_pass" v-model="form.password" type="password" placeholder=""
                            :state="validateState('password')" aria-describedby="feedback_2">
                        </b-form-input>
                        <b-form-invalid-feedback id="feedback_2">This is a required field.</b-form-invalid-feedback>
                    </b-form-group>

                    <div class="">
                        <b-row align-h="center">
                            <b-button type="submit" variant="primary">Login</b-button>
                            <!-- <b-button type="reset" variant="danger">Reset</b-button> -->
                        </b-row>

                    </div>
                    <div class="pt-2">
                        
                        <b-row align-h="around">
                            
                                <b-button v-b-tooltip.hover.bottom="'Back to Home'" variant="secondary">
                                    <NuxtLink class="" :to="{ name: 'index' }">
                                        <b-icon icon="house-door-fill" variant="secondary" font-scale="1"></b-icon>
                                    </NuxtLink>
                                </b-button>
                            
                                <b-button v-b-tooltip.hover.bottom="'Click here to register'" variant="secondary">
                                    <NuxtLink class="" :to="{ name: 'register_page' }">
                                        <b-icon icon="pencil-square" variant="secondary" font-scale="1"></b-icon>
                                    </NuxtLink>
                                </b-button>
                           
                        </b-row>
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
import { required, email } from "vuelidate/lib/validators";


export default {
    name: 'LoginForm',
    components: {

    },
    mixins: [validationMixin],
    emits: ['login_user'],
    data() {
        return {
            form: {
                emailaddress: '',
                password: ''
            },
        };
    },
    validations: {
        form: {
            emailaddress: {
                required,
                email
            },
            password: {
                required,
            }
        }
    },
    methods: {
        validateState(emailaddress) {
            const { $dirty, $error } = this.$v.form[emailaddress];
            return $dirty ? !$error : null;
        },
        onSubmit(event) {
            event.preventDefault()

            this.$v.form.$touch();
            if (this.$v.form.$anyError) {
                return;
            }
            this.$emit('login-user', loginData)
            console.log('logindata:')
            console.log(loginData)
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