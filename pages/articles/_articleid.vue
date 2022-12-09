<template>
    <div v-if="(this.articlesData == '')" class="bg-zinc-800 min-h-screen text-white">
        <FailedArticlePage />
    </div>

    <div v-else class="bg-zinc-800 min-h-screen text-white">
        <NavBar />
        <!-- <b-container>
            <b-row>
                <b-col cols="9" xl="10" lg="10" md="10" sm="10">
                    <SlideBar class="ml-2 mt-20 xs:ml-20 sm:ml-20 md:ml-20 lg:ml-20 xl:ml-20" />
                </b-col>

            </b-row>
        </b-container> -->

        <div class="pt-28 flex justify-center">

            <b-col cols="12" xl="12" lg="12" md="12" sm="12">
                <b-container style="max-width: 1000px;" class="">

                    <div v-for="(a, index ) in article" :key="index" class="flex justify-between mb-2">
                        <b-card-text class="text-2xl mb-2 font-bold break-words">
                            {{ a.movie_name }}
                        </b-card-text>
                        <div v-if="(a.user_user_id == userID || userRole == 2)"
                            v-b-tooltip.hover.top="'Delete article.'">
                            <b-button variant="outline-danger" class="" @click="confirm(a.article_id)">
                                <b-icon icon="trash-fill" variant="" font-scale="1" class=""></b-icon>
                            </b-button>
                        </div>
                    </div>

                    <div v-for="(a, index ) in article" :key="index">
                        <b-card border-variant="primary" bg-variant="dark">
                            <b-card-text class="text-lg break-words text-justify">
                                {{ a.articles }}
                            </b-card-text>
                            <b-card-text class="text-sm mt-2">
                                Writer: {{ a.writer }}
                            </b-card-text>
                            <!-- <b-card-text class="text-sm">
                                Movie name: {{ a.movie_name }}
                            </b-card-text> -->
                        </b-card>
                    </div>

                    <b-card border-variant="primary" bg-variant="dark" class="my-8 overflow-y-scroll"
                        style="height: 380px;">
                        <div v-if="articleComment == ''" class="flex justify-center">
                            <p class="text-2xl font-bold mt-20">This article currently have no comments.</p>
                        </div>
                        <b-row align-h="center" class="w-auto">
                            <b-col cols="12" xl="12" lg="12" md="12" sm="12">
                                <div v-for="(ac, index ) in articleComment" :key="index">
                                    <b-card :title="ac.comment_writer" tag="article" style="" class="mb-2 text-xs"
                                        bg-variant="secondary" text-variant="light" border-variant="primary">
                                        <b-card-text class="text-sm break-words">: {{ ac.comment }}</b-card-text>

                                        <div class="absolute top-4 right-4" v-if="ac.user_user_id == userID">
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
                                </div>
                            </b-col>
                        </b-row>
                    </b-card>

                </b-container>
                <b-container v-if="userRole == 1 || userData == null" style="max-width: 1000px;" class="pb-8">

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
                                    <div v-if="userData == null" v-b-tooltip.hover.bottom="'Please Login.'">
                                        <b-button disabled type="submit" variant="primary" size="sm">
                                            <b-icon icon="arrow-return-right" variant="light" font-scale="1"></b-icon>
                                        </b-button>
                                    </div>

                                    <div v-if="userRole == 1">
                                        <b-button type="submit" variant="primary" size="sm">
                                            <b-icon icon="arrow-return-right" variant="light" font-scale="1"></b-icon>
                                        </b-button>
                                    </div>
                                </b-col>
                            </b-row>

                        </b-form>

                    </b-card>

                </b-container>
            
                
                    <b-container v-if="userRole == 2">                        
                    </b-container>
                
            </b-col>

        </div>


    </div>
</template>
    
<script>
import swal from 'sweetalert2/dist/sweetalert2.js'
import { validationMixin } from "vuelidate";
import { required, maxLength } from "vuelidate/lib/validators";
import axios from "axios"
// import SlideBar from '@/components/slide_bar.vue'
import NavBar from '@/components/nav_bar.vue'
import FailedArticlePage from '@/components/failed_article_page.vue';

export default {
    name: 'ShowArticle',
    components: {
        NavBar,
        FailedArticlePage
    },

    mixins: [validationMixin],
    data() {
        return {
            form: {
                comment: '',
            },
            titleById: '',
            userID: '',
            userData: null,
            userRole: '',
            userName: '',
            article: '',
            articleComment: [],
            articleId: '',
            articlesData: '',
            editCommetID: '',
            editMode: false,
            url: 'http://localhost:3000'
            // url: 'https://backend-final.azurewebsites.net'
        }
    },
    async created() {
        await this.getSingleArticle();
        await this.getCommentArticle();
    },
    async mounted() {
        if (document.cookie == null) { return }

        try {
            const res = await fetch(this.url + "/getsingleuser", {
                headers: {
                    'Content-type': 'application/json'
                },
                credentials: 'include',
                withCredentials: true
            })
            const getuserdata = await res.json()
            this.userData = getuserdata
            console.log('Userdata:')
            console.log(this.userData)
            this.userRole = getuserdata.data.role
            console.log('Userrole:')
            console.log(this.userRole)
            this.userID = getuserdata.data.user_id
            console.log('UserID:')
            console.log(this.userID)
            this.userName = getuserdata.data.name
            console.log('UserName:')
            console.log(this.userName)

        }
        catch (error) {
            console.log(`get user failed: ${error}`)
        }
    },
    // async fetch() {
    //     await this.getSingleArticle();
    //     await this.getCommentArticle();
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
            this.article = result.data;

            console.log('Getsinglearticle:')
            console.log(this.article)
            for (const i in this.article) {
                this.articlesData = this.article[i].article_id
                console.log('ArticleData:')
                console.log(this.articlesData)
            }
            if (this.articlesData == '') {
                this.$router.push({ name: 'index' })
                setTimeout(() => {
                    console.log('Redirect2!')

                    swal.fire({
                        title: 'This article does not exist.',
                        // text: 'Do you want to continue',
                        icon: 'error',
                        confirmButtonText: 'Done',
                        confirmButtonColor: '#007bff'
                    })
                }, 1000);

            }
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
                    credentials: 'include',
                    body: JSON.stringify({
                        comment: this.form.comment,
                        user_user_id: this.userID,
                        comment_writer: this.userName,
                        article_user_user_id: this.article[0].user_user_id,
                        article_article_id: this.$route.params.articleid
                    })
                })
                console.log('Post Your comment success')
                this.form.comment = ''
                this.$nextTick(() => {
                    this.$v.$reset();
                })
                await this.getCommentArticle();
            }
            catch (error) {
                console.log(`addArticle False!!! ${error}`)
            }
        },

        // CONFIRM
        confirm(articleId) {
            swal.fire({
                title: 'Are you sure?',
                text: "You will be deleting this article!",
                icon: 'warning',
                showCancelButton: true,
                confirmButtonColor: '#007bff',
                cancelButtonColor: '#dc2626',
                confirmButtonText: 'Confirm',
                cancelButtonText: 'Cancel'
            }).then((result) => {
                if (result.isConfirmed) {
                    this.deleteArticle(articleId)
                }
            })
        },

        // DELETE
        async deleteComment(commentId) {
            try {
                await fetch(`${this.url}/deletecomment/${commentId}`, {
                    method: 'DELETE',
                    credentials: 'include'
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

        async deleteArticle(articleId) {
            console.log("Delete ID")
            console.log(articleId)

            try {
                await fetch(`${this.url}/deletearticle/${articleId}`, {
                    method: 'DELETE',
                    credentials: 'include'
                })
            }
            catch (error) {
                console.log(`delete failed: ${error}`)
            }
            swal.fire({
                title: 'This article has been deleted.',
                // text: 'Do you want to continue',
                icon: 'success',
                confirmButtonText: 'Done',
                confirmButtonColor: '#007bff'
            })
            setTimeout(() => { this.$router.go(-1) }, 1000);
            setTimeout(() => { this.$router.go(0) }, 2000);

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