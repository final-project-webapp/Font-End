<template>

    <div class="bg-zinc-800 min-h-screen text-white">
        <b-container>
            <b-row>
                <b-col cols="9" xl="10" lg="10" md="10" sm="10">
                    <SlideBar class="ml-2 pt-20 xs:pl-20 sm:pl-20 md:pl-20 lg:pl-20 xl:pl-20" />
                </b-col>

            </b-row>
        </b-container>

        <div class="mt-20 flex justify-center">

            <b-col cols="12" xl="12" lg="12" md="12" sm="12">
                <b-container style="max-width: 1000px;" class="mb-8">

                    <div v-for="(a, index ) in article" :key="index">
                        <b-card border-variant="primary" bg-variant="secondary">
                            <b-card-text class="text-lg">
                                {{ a.articles }}
                            </b-card-text>
                            <!-- <b-card-text class="text-lg">
                                {{ a.article_id }}
                            </b-card-text> -->
                        </b-card>
                    </div>

                    <b-card border-variant="primary" bg-variant="dark" class="mt-8 overflow-y-scroll"
                        style="height: 380px;">
                        <div v-if="articleComment == ''" class="flex justify-center">
                            <p class="text-2xl">No Comment Yet.</p>
                        </div>
                        <b-row align-h="center" class="w-auto">
                            <b-col cols="12" xl="12" lg="12" md="12" sm="12">
                                <div v-for="(ac, index ) in articleComment" :key="index">
                                    <b-card :title="ac.name" tag="article" style="" class="mb-2 text-xs"
                                        bg-variant="secondary" text-variant="light" border-variant="primary">


                                        <b-card-text class="text-sm">: {{ ac.comment }}</b-card-text>
                                        <b-card-text class="text-sm">ID: {{ ac.comment_id }}</b-card-text>
                                        <b-card-text class="text-sm">WritterID: {{ ac.user_id }}</b-card-text>

                                        <div class="absolute top-4 right-4" v-if="ac.user_id == userID">
                                            <b-dropdown size="sm" no-caret>
                                                <template #button-content>
                                                    <b-icon icon="three-dots-vertical" variant="light" font-scale="1">
                                                    </b-icon>
                                                </template>
                                                <b-dropdown-item-button variant="dark" class="text-xs"
                                                    @click="deleteComment(ac.comment_id)">
                                                    <b-icon icon="trash-fill" variant="dark" font-scale="1"
                                                        class="flex justify-end"></b-icon>
                                                    Delete
                                                </b-dropdown-item-button>
                                                <b-dropdown-item-button variant="dark" class="text-xs"
                                                    @click="showCommentInfo(ac)">
                                                    <b-icon icon="pencil-square" variant="dark" font-scale="1"
                                                        class="flex justify-end"></b-icon>
                                                    Edit
                                                </b-dropdown-item-button>
                                            </b-dropdown>
                                        </div>

                                        <div class="absolute top-4 right-4" v-if="userRole == 2">
                                            <b-dropdown size="sm" no-caret>
                                                <template #button-content>
                                                    <b-icon icon="three-dots-vertical" variant="light" font-scale="1">
                                                    </b-icon>
                                                </template>
                                                <b-dropdown-item-button variant="dark" class="text-xs"
                                                    @click="deleteComment(ac.comment_id)">
                                                    <b-icon icon="trash-fill" variant="dark" font-scale="1"
                                                        class="flex justify-end"></b-icon>
                                                    Delete
                                                </b-dropdown-item-button>                                                
                                            </b-dropdown>
                                        </div>

                                    </b-card>

                                    <!-- <b-card :title="ac.name" tag="article" style="" class="mb-2 text-xs"
                                        bg-variant="secondary" text-variant="primary" border-variant="primary" v-if="ac.role == 2">


                                        <b-card-text class="text-sm">: {{ ac.comment }}</b-card-text>
                                        <b-card-text class="text-sm">ID: {{ ac.comment_id }}</b-card-text>
                                        <b-card-text class="text-sm">WritterID: {{ ac.user_id }}</b-card-text>

                                        <div class="absolute top-4 right-4" v-if="ac.user_id == userID">
                                            <b-dropdown size="sm" no-caret>
                                                <template #button-content>
                                                    <b-icon icon="three-dots-vertical" variant="light" font-scale="1">
                                                    </b-icon>
                                                </template>
                                                <b-dropdown-item-button variant="dark" class="text-xs"
                                                    @click="deleteComment(ac.comment_id)">
                                                    <b-icon icon="trash-fill" variant="dark" font-scale="1"
                                                        class="flex justify-end"></b-icon>
                                                    Delete
                                                </b-dropdown-item-button>
                                                <b-dropdown-item-button variant="dark" class="text-xs"
                                                    @click="showCommentInfo(ac)">
                                                    <b-icon icon="pencil-square" variant="dark" font-scale="1"
                                                        class="flex justify-end"></b-icon>
                                                    Edit
                                                </b-dropdown-item-button>
                                            </b-dropdown>
                                        </div>

                                        <div class="absolute top-4 right-4" v-if="userRole == 2">
                                            <b-dropdown size="sm" no-caret>
                                                <template #button-content>
                                                    <b-icon icon="three-dots-vertical" variant="light" font-scale="1">
                                                    </b-icon>
                                                </template>
                                                <b-dropdown-item-button variant="dark" class="text-xs"
                                                    @click="deleteComment(ac.comment_id)">
                                                    <b-icon icon="trash-fill" variant="dark" font-scale="1"
                                                        class="flex justify-end"></b-icon>
                                                    Delete
                                                </b-dropdown-item-button>                                                
                                            </b-dropdown>
                                        </div>

                                    </b-card> -->
                                </div>
                            </b-col>
                        </b-row>
                    </b-card>

                </b-container>
                <b-container style="max-width: 1000px;">

                    <b-card border-variant="primary" bg-variant="dark" class="">

                        <b-form @submit.prevent="onSubmit">

                            <b-row align-h="center" align-v="center">
                                <b-col cols="9" xl="11" lg="11" md="10" sm="10">
                                    <b-form-textarea id="input_comment" v-model="form.comment" type="text"
                                        placeholder="Comment" size="sm" rows="2" max-rows="3" style="max-width: 1000px;"
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
                <!-- <div>
                    <CF class="mt-8" @comment-data="addComment"
                    v-bind:oldCommentData="articleComment" />
                </div> -->
            </b-col>

        </div>


    </div>
</template>
    
<script>
import { validationMixin } from "vuelidate";
import { required, maxLength } from "vuelidate/lib/validators";
import axios from "axios"
import SlideBar from '@/components/slide_bar.vue'

export default {
    name: 'ShowArticle',
    components: {
        SlideBar,
    },

    mixins: [validationMixin],
    data() {
        return {
            form: {
                comment: '',
            },
            userID: '',
            userData: null,
            userRole: '',
            article: '',
            articleComment: [],
            articleId: '',
            editCommetID: '',
            editMode: false,
            // url: 'http://localhost:3000'
            url: 'https://mediare.azurewebsites.net'
        }
    },
    async mounted() {
        if (document.cookie == null) { return }

        try {
            console.log('Process 2:')
            const res = await fetch(this.url + "/getsingleuser", {
                headers: {
                    'Content-type': 'application/json'
                },
                credentials: 'include'
            })
            const getuserdata = await res.json()
            this.userData = getuserdata
            console.log('Userdata:')
            console.log(this.userData)
            console.log('Process 3:')
            this.userRole = getuserdata.data.role
            console.log('Userrole:')
            console.log(this.userRole)
            this.userID = getuserdata.data.user_id
            console.log('UserID:')
            console.log(this.userID)

        }
        catch (error) {
            console.log(`get user failed: ${error}`)
        }
    },
    async fetch() {
        await this.getSingleArticle();
        await this.getCommentArticle()
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

        onSubmit() {
            if (this.editMode) {
                this.editComment()
            } else {
                this.postComment()
            }
        },

        // GET
        async getSingleArticle() {
            const data = axios.get(`${this.url}/getsinglearticle/${this.$route.params.articleid}`)
            const result = await data;
            console.log('single result:')
            console.log(data)

            this.article = result.data;

            console.log('singlearticle:')
            console.log(this.article)
        },

        async getCommentArticle() {
            const data = axios.get(`${this.url}/getcommentinarticle/${this.$route.params.articleid}`)
            const result = await data;
            this.articleComment = result.data;
            this.articleId = this.$route.params.articleid

            console.log('Comment Article:')
            console.log(this.articleComment)
        },

        // Post
        async postComment() {
            this.$v.form.$touch();
            if (this.$v.form.$anyError) {
                return;
            }
            try {
                console.log('POST')
                await fetch(this.url + "/addcomment", {
                    method: 'POST',
                    headers: {
                        'Content-type': 'application/json'
                    },
                    body: JSON.stringify({
                        comment: this.form.comment,
                        user_user_id: this.userID,
                        article_user_user_id: this.article[0].user_user_id,
                        article_article_id: this.$route.params.articleid
                    })
                })
                console.log('Post Your comment success')
                this.form.comment = ''
                this.$nextTick(() => {
                    this.$v.$reset();
                });
                const data = axios.get(`${this.url}/getcommentinarticle/${this.articleId}`)
                const result = await data;
                console.log('Result')
                console.log(result)

                this.articleComment = result.data;
            }
            catch (error) {
                console.log(`addArticle False!!! ${error}`)
            }
        },

        // DELETE
        async deleteComment(commentId) {
            try {
                await fetch(`${this.url}/deletecomment/${commentId}`, {
                    method: 'DELETE',
                })
                const data = axios.get(`${this.url}/getcommentinarticle/${this.articleId}`)
                const result = await data;
                console.log('DeleteResult')
                console.log(result)

                this.articleComment = result.data;
                console.log('Your comment delete success')

            }
            catch (error) {
                console.log(`delete failed: ${error}`)
            }
        },

        // EDIT
        showCommentInfo(commentInfo) {
            console.log('commentInfo')
            console.log(commentInfo)
            this.editMode = true
            this.editCommentID = commentInfo.comment_id
            this.form.comment = commentInfo.comment
        },
        async editComment() {
            try {
                await fetch(this.url + "/editcomment", {
                    method: 'PUT',
                    headers: {
                        'Content-type': 'application/json'
                    },
                    credentials: 'include',
                    body: JSON.stringify({
                        comment_id: this.editCommentID,
                        comment: this.form.comment
                    }),
                })
                const data = axios.get(`${this.url}/getcommentinarticle/${this.$route.params.articleid}`)
                const result = await data;
                this.articleComment = result.data;
                this.editMode = false
                this.editCommentID = ''
                this.form.comment = ''
                this.$nextTick(() => {
                    this.$v.$reset();
                });

            }
            catch (error) {
                console.log(`edit failed: ${error}`)
            }            
        },

        async reloadComment() {
            this.articleComment = await this.getCommentArticle()
        }
    }
}
</script>