<template>
    <section class="flex flex-col items-center gap-2 max-w-[275px]">
        <img src="/public/ai-svgrepo-com.svg" class="w-32 h-32 rounded-full" />
        <div class="flex flex-col items-center">
            <h1 class="text-center font-bold text-xl">
               <span class="text-slate-500">GPT</span> Maya
            </h1>
            <small class="text-slate-500">(Nuxt.js, TypeScript and Open AI)</small>
        </div>
        <form @submit.prevent="handleSubmit">
            <fieldset :disabled="isSubmitted" class="flex flex-col gap-4 w-full w-full">
                <div class="flex flex-col">
                    <input
                        v-model.trim="customerName"
                        type="text"
                        placeholder="Your name"
                        class="w-full transition p-3 text-sm border border-slate-300/60 rounded-md shadow-sm placeholder:text-slate-400/90 focus:ring-2 focus:ring-slate-300"
                    />
                    <small class="text-slate-500 italic mt-1">
                        Будь ласка, вкажіть ім'я, щоб Майя знала, з ким розмовляє. 
                    </small>
                </div>
                <button :disabled="hasNameError"
                    type="submit"
                    class="transition w-full bg-slate-700 text-white font-medium p-3 rounded-md hover:bg-opacity-90">
                    Почати діалог
                </button>
                <button v-if="thread && run"
                    @click="isChatting = true"
                    type="button"
                    class="transition w-full bg-slate-300 text-gray font-medium p-3 rounded-md hover:bg-opacity-90">
                    Повернутись до минулого діалогу
                </button>
            </fieldset>
        </form>
    </section>
</template>

<script setup lang="ts">
const isChatting = useIsChatting();
const { customerName, hasNameError } = useCustomer();

const thread = useCookie("thread-id");
const run = useCookie("run-id");

const isSubmitted = ref(false);

async function handleSubmit() {
    // if (hasNameError.value) {
    //     console.log('hasNameError:', hasNameError.value);
    //     // console.error('Ошибка: имя клиента слишком короткое');
    //     return;
    // } else {
    //     console.log('hasNameError:', hasNameError.value);
    // }

    isSubmitted.value = true;

    const response = await $fetch("/api/thread", {
        query: {
            customer: customerName.value,
        }
    });

    thread.value = response.thread;
    run.value = response.run;

    isChatting.value = true;
}
</script>