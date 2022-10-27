<template>
    <div>
        <b-container style="max-width: 1000px;">

            <b-card border-variant="primary" bg-variant="dark" class="">

                <b-form @submit.prevent="onSubmit">

                    <b-row align-h="center" align-v="center">
                        <b-col cols="9" xl="11" lg="11" md="10" sm="10">
                            <b-form-textarea id="input_comment" v-model="form.comment" type="text" placeholder="Comment"
                                size="sm" rows="2" max-rows="3" style="max-width: 1000px;"
                                :state="validateState('comment')" aria-describedby="feedback_1">
                            </b-form-textarea>
                            <b-form-invalid-feedback id="feedback_1">This is a required field and not more than
                                1000
                                characters.
                            </b-form-invalid-feedback>
                        </b-col>
                        <b-col>
                            <b-button type="submit" variant="primary" size="sm">
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
    emits: ['oldcomment-data'],
    props: ['oldCommentData'],
    mixins: [validationMixin],
    data() {
        return {
            form: {
                comment: '',
            },
            defaultview: 1,
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
        validateState(comment) {
            const { $dirty, $error } = this.$v.form[comment];
            return $dirty ? !$error : null;
        },
        showCommentInfo(oldCommentInfo) {
            console.log('oldCommentData:')
            console.log(oldCommentInfo)
            this.form.comment = oldCommentInfo.comment
        },
        onSubmit() {

            this.$v.form.$touch();
            if (this.$v.form.$anyError) {
                return;
            }

            const commentData = {
                comment: this.form.comment,
                // user_user_id: this.user_user_id
            }
            this.$emit('comment-data', commentData)
            // console.log('comment data:')
            // console.log(commentData)
            // console.log('Form:')
            // console.log(this.form.comment)          

            this.form.comment = ''
            this.$nextTick(() => {
                this.$v.$reset();
            });



        },
    }
}
</script>