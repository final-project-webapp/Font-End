<template>
    <div class="pt-20 flex">
        <b-container>
            <b-card border-variant="primary" bg-variant="dark">
                <b-form @submit="onSubmit" @reset="onReset">
                    <b-form-group id="input_group_1" label="Discuss Title:" label-for="input_1">
                        <b-form-input id="input_1" v-model="form.title" type="text" placeholder="" :state="validateState('title')" aria-describedby="input-1-live-feedback">
                        </b-form-input>
                        <b-form-invalid-feedback id="input-1-live-feedback">This is a required field.</b-form-invalid-feedback>
                    </b-form-group>

                    <b-form-group id="input_group_2" label="Writer Name:" label-for="input_2">
                        <b-form-input id="input_2" v-model="form.name" type="text" placeholder="" :state="validateState('name')" aria-describedby="input-2-live-feedback">
                        </b-form-input>
                        <b-form-invalid-feedback id="input-2-live-feedback">This is a required field and must be at
                            least 3 characters.</b-form-invalid-feedback>

                    </b-form-group>



                    <b-form-group id="input_group_3" label="Discuss Article:" label-for="input_3">
                        <b-form-textarea id="input_3" v-model="form.article" placeholder="" rows="8">
                        </b-form-textarea>
                    </b-form-group>

                    <b-form-group id="input_group_4" v-slot="{ ariaDescribedby }" label="Language:">
                        <b-form-checkbox-group id="checkbox_group_1" v-model="form.lang"
                            :aria-describedby="ariaDescribedby">
                            <b-form-checkbox value="TH">TH</b-form-checkbox>
                            <b-form-checkbox value="EN">EN</b-form-checkbox>
                        </b-form-checkbox-group>
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
            this.$v.form.$touch();
            if (this.$v.form.$anyError) {
                return;
            }

            event.preventDefault()
            alert(JSON.stringify(this.form))
        },
        onReset(event) {
            event.preventDefault()
            // Reset our form values
            this.form.title = ''
            this.form.name = ''
            this.form.article = ''
            this.form.checked = []
            // Trick to reset/clear native browser form validation state
            this.$nextTick(() => {
                this.$v.$reset();
            });
        }
    }
}
</script>