<!-- <script setup lang="ts">
const url = ref("")
const isInput = ref(true)
// const currentHref = ref('https://short.inedible.dev/')
const currentHref = useState('currentHref')
const appName = useState('appName')


// const data = await $fetch.raw("/api/newUrl", {
//     method: "POST",
//     body: { id: '2' }
// })
// console.log(data._data)
// if (data.status !== 200) {
//     console.log('url not found')
// }

const generateRandomUrl = (length?: number): string => {
    const characters = 'ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789-_';
    const charactersLength = characters.length;
    const urlLength = length ?? Math.floor(Math.random() * 3) + 6; // Random length between 6 and 8

    let result = '';
    for (let i = 0; i < urlLength; i++) {
        const randomIndex = Math.floor(Math.random() * charactersLength);
        result += characters.charAt(randomIndex);
    }
    return result;
}
const isValidURL = (url: string): boolean => {
    const urlPattern = /^(https?|ftp):\/\/[^\s/$.?#].[^\s]*$/i;
    return urlPattern.test(url);
}

const onShorten = async () => {
    if (!isValidURL(url.value)) {
        alert("Not a valid URL")
        return;
    }
    let urlString = ""
    while (true) {
        urlString = generateRandomUrl()
        const data = await $fetch.raw("/api/checkUrl", {
            method: "POST",
            body: { id: `${urlString}` }
        })
        if (data._data == "not found") {
            console.log(data._data)
            try {
                const newData = await $fetch.raw("/api/newUrl", {
                    method: "POST",
                    body: { id: `${urlString}`, url: `${url.value}` }
                })
                console.log(newData)
            } catch (err: any) {
                alert(err.message)
            }
            isInput.value = false
            url.value = `${urlString}`
            break;
        }
    }
}

const onCopy = () => {
    navigator.clipboard.writeText(`${currentHref.value}${url.value}`)
    alert(`Copied ${currentHref.value}${url.value} to clipboard.`)
}
</script> -->
<script lang="ts">
import { getAuth, onAuthStateChanged } from 'firebase/auth';
export default {
    // mounted() {
    //     const isSubdomain = (url: any, domain: any) => {
    //         try {
    //             const hostname = new URL(url).hostname;
    //             return hostname.endsWith('.' + domain) && hostname.split('.').length > domain.split('.').length;
    //         } catch (e) {
    //             // Handle invalid URLs
    //             return false;
    //         }
    //     }
    //     const currentHref = useState('currentHref')
    //     currentHref.value = window.location.href
    //     const appName = useState('appName')
    //     if (isSubdomain(currentHref.value, "inedible.dev")) {
    //         appName.value = "Inedible"
    //     } else {
    //         appName.value = "Puntawat"
    //     }
    // },
    async beforeMount() {
        const app = useFirebaseApp();
        const auth = getAuth();
        const useUID = useState<string | undefined>("uid");
        onAuthStateChanged(auth, async (user) => {
            if (!(user?.uid)) {
                await navigateTo("/signup");
            } else {
                useUID.value = user.uid
                await navigateTo("/dash");
            }
        })
    },
}
</script>

<template>
    <!-- <div class="text-center justify-center flex flex-col items-center h-screen content-center object-center">
        <h1 class="text-[35px] font-bold mb-[30px]">{{ appName }}'s URL Shortener</h1>

        <div class="flex-row flex mx-auto">
            <input type="text" id="first_name"
                class="bg-gray-50 border border-gray-300 outline-none mr-[15px] text-gray-900 text-sm rounded-lg focus:ring-sky-500 focus:border-sky-500 block w-full p-2.5"
                placeholder="URL" name="url" v-model="url" v-if="isInput" required autocomplete="off" autofocus>
            <div type="text"
                class="bg-gray-50 border pt-[12px] border-gray-300 outline-none mr-[15px] text-gray-900 text-sm rounded-lg focus:ring-sky-500 focus:border-sky-500 block w-full p-2.5"
                id="urlField" v-if="!isInput">
                {{ currentHref }}{{ url }}</div>
            <button
                class="outline-none transition-all ease-in-out hover:scale-110 focus:outline-none focus:ring-2 focus:ring-slate-400 focus:ring-offset-2 focus:ring-offset-slate-50 text-white font-semibold h-12 px-6 rounded-lg w-full flex items-center justify-center sm:w-auto bg-sky-500 dark:highlight-white/20 hover:bg-sky-400"
                type="submit" @click="onShorten" v-if="isInput">Shorten</button>
            <button
                class="outline-none transition-all ease-in-out hover:scale-110 focus:outline-none focus:ring-2 focus:ring-slate-400 focus:ring-offset-2 focus:ring-offset-slate-50 text-white font-semibold h-12 px-6 rounded-lg w-full flex items-center justify-center sm:w-auto bg-sky-500 dark:highlight-white/20 hover:bg-sky-400"
                type="submit" @click="onCopy" v-if="!isInput">Copy</button>
        </div>
    </div> -->
    <!-- Please enable JavaScript -->
</template>