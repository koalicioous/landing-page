<template>
    <div class="fixed top-0 left-0 w-full h-screen">
        <div class="sticky top-0 w-full h-screen bg-gray-900 left-0 bg-opacity-80 backdrop-filter backdrop-blur-sm flex items-center justify-center z-20 px-2">
            <div class="bg-white rounded-md p-8 max-w-md">
                <div v-if="!submitted">
                    <button type="button" class="transform rotate-45 bg-gray-100 w-8 h-8 rounded-full flex items-center justify-center float-right -mt-5 -mr-5 hover:bg-gray-200 transition-all" @click="$emit('close')"><span>+</span></button>
                    <div v-if="!loading">
                        <h1 class="text-center font-bold text-gray-700">SUBSCRIBE TO OUR PROGRESS</h1>
                        <h2 class="text-center text-gray-600">Join us on our journey to digitalize parking system in Indonesia.</h2>
                    </div>
                    <form action="" v-on:submit.prevent="postData()" v-if="!loading">
                        <input type="text" name="name" id="name" placeholder="Your name" class="px-2 py-3 rounded-md border border-gray-300 w-full mt-4" v-model="name">
                        <input type="email" name="email" id="email" placeholder="Your email address" class="px-2 py-3 rounded-md border border-gray-300 w-full mt-3" v-model="email">
                        <button type="submit" class="p-3 text-center rounded-md bg-blue-600 text-white w-full mt-3">
                            SUBMIT
                        </button>
                    </form>
                    <div class="text-center" v-if="loading">
                        Loading..
                    </div>
                </div>
                <div class="flex flex-col items-center" v-if="submitted && !loading">
                    <img
                        src="~/assets/check-one.svg"
                        class="rounded-lg"
                        alt=""
                    >
                    <div>
                        <h1 class="text-center font-bold text-gray-700 mt-3">SUCCESS!</h1>
                        <h2 class="text-center text-gray-600">Thank you for your interest on our journey. We will send you more reports of our progress in the futures.</h2>
                    </div>
                    <NuxtLink to="/" class="p-3 rounded-md bg-blue-600 w-full text-white font-semibold text-center mt-3">
                        HOMEPAGE
                    </NuxtLink>
                    <button class="mt-2 rounded-md border border-gray-400 text-gray-700 p-3 w-full text-center" @click="$emit('close')">
                        CLOSE
                    </button>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    data() {
        return {
            name: '',
            email: '',
            isSubmitted: false,
            loading: false,
            submitted: false,
        }
    },
    methods: {
        async postData() {
            try {
                this.loading = true
                const newSubscriber = this.$fire.firestore.collection('subscribers').doc()
                const data = await newSubscriber
                .set({
                    name: this.name,
                    email: this.email
                })
                .then(() => {
                    this.email = ''
                    this.name = ''
                    this.submitted = true
                    this.loading = false
                })
            }
            catch(err) {
                console.log(err)
                this.loading = false
            }
        }
    }
}
</script>