<template>
    <div>
        <b-container style="max-width: 1000px;">

            <b-card border-variant="primary" bg-variant="dark" class="">

                <b-form @submit.prevent="onSubmit">

                    <b-row align-h="center" no-gutters>
                        <b-col cols="9" xl="11" lg="11" md="10" sm="10">
                            <b-form-input id="input_comment" v-model="form.comment" type="text" placeholder="Comment"
                                class="rounded-none" squared size="sm" style="max-width: 1000px;"
                                :state="validateState('comment')" aria-describedby="feedback_comment">
                            </b-form-input>
                            <b-form-invalid-feedback id="feedback_2">This is a required field and not more than
                                1000
                                characters.
                            </b-form-invalid-feedback>
                        </b-col>
                        <b-col>
                            <b-button type="submit" variant="primary" squared size="sm">
                                <b-icon icon="arrow-return-right" variant="light" font-scale="1"></b-icon>
                            </b-button>
                        </b-col>
                    </b-row>

                </b-form>

            </b-card>

        </b-container>
    </div>
</template>

<script>
import { validationMixin } from "vuelidate";
import { required, maxLength } from "vuelidate/lib/validators";

export default {

    name: 'CommentForm',
    components: {

    },
    mixins: [validationMixin],
    data() {
        return {
            form: {
                comment: '',
            },
            defaultview: 1,
            testuser: 1
        };
    },
    validations: {
        form: {
            comment: {
                required,
                maxLength: maxLength(1000)
            },
        }

    },
    methods: {
        validateState() {
            const { $dirty, $error } = this.$v.form;
            return $dirty ? !$error : null;
        },
        onSubmit() {
            // event.preventDefault()

            this.$v.form.$touch();
            if (this.$v.form.$anyError) {
                return;
            }

            const commentData = {
                comment: this.form.comment,
                userID: this.testuser
            }
            this.$emit('comment-data', commentData)
            console.log('comment data:')
            console.log(commentData)
            console.log('Form:')
            console.log(this.form.comment)


            alert(JSON.stringify(this.form))

            this.form.comment = ''
            this.$nextTick(() => {
                this.$v.$reset();
            });

        },
    }
}
</script>