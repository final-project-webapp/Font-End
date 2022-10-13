<template>
    <div>
        <b-container style="max-width: 1000px;">

            <b-card border-variant="primary" bg-variant="dark" class="">

                <b-form @submit.prevent="onSubmit">

                    <b-row align-h="center" no-gutters>
                        <b-col cols="9" xl="11" lg="11" md="10" sm="10">
                            <b-form-input id="input_comment" v-model="form.comment" type="text" placeholder="Comment"
                                class="rounded-none" squared size="sm" style="max-width: 1000px;"
                                :state="validateState('comment')" aria-describedby="feedback_1">
                            </b-form-input>
                            <b-form-invalid-feedback id="feedback_1">This is a required field and not more than
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
            // userID: '',
            // userData: null,
            // userRole: '',
            defaultview: 1,
            // user_user_id: 1
        };
    },
    // async mounted() {
    //     console.log('Process 1:')
    //     console.log(this.userData)
    //     if (document.cookie == null) { return }

    //     try {
    //         console.log('Process 2:')
    //         const res = await fetch(this.url + "/getsingleuser", {
    //             headers: {
    //                 'Content-type': 'application/json'
    //             },
    //             credentials: 'include'
    //         })
    //         const getuserdata = await res.json()
    //         this.userData = getuserdata
    //         console.log('Userdata:')
    //         console.log(this.userData)
    //         console.log('Process 3:')
    //         this.userRole = getuserdata.data.role
    //         console.log('Userrole:')
    //         console.log(this.userRole)
    //         this.userID = getuserdata.data.user_id
    //         console.log('UserID:')
    //         console.log(this.userID)
    //         // return getuserdata
    //     }
    //     catch (error) {
    //         console.log(`get user failed: ${error}`)
    //     }
    // },
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

            // alert(JSON.stringify(this.form))
            // alert('Post Success')

            this.form.comment = ''
            this.$nextTick(() => {
                this.$v.$reset();
            });



        },
    }
}
</script>