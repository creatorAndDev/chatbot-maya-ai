<template>
	<form @submit.prevent="handleSubmit" class="relative border-slate-300 border-t" style="margin: 0 -1.5rem -1.5rem;">
		<fieldset :disabled="isSubmitted">
			<textarea
				v-model.trim="newMessage"
				@keypress.enter.prevent="handleSubmit" style="display: block;"
				class="transition py-4 px-5 w-full text-sm border-none placeholder:text-slate-400/90"
				placeholder="Enter your message..."
			></textarea>
			<button class="flex items-center justify-center absolute top-4 right-2 flex items-center from-sky-500 bg-slate-600 w-10 h-10 overflow-hidden rounded-full">
				<svg fill="#fff" height="18" width="18" version="1.1" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" viewBox="0 0 495.003 495.003" xml:space="preserve">
					<g>
						<path d="M164.711,456.687c0,2.966,1.647,5.686,4.266,7.072c2.617,1.385,5.799,1.207,8.245-0.468l55.09-37.616   l-67.6-32.22V456.687z"/>
						<path d="M492.431,32.443c-1.513-1.395-3.466-2.125-5.44-2.125c-1.19,0-2.377,0.264-3.5,0.816L7.905,264.422   c-4.861,2.389-7.937,7.353-7.904,12.783c0.033,5.423,3.161,10.353,8.057,12.689l125.342,59.724l250.62-205.99L164.455,364.414   l156.145,74.4c1.918,0.919,4.012,1.376,6.084,1.376c1.768,0,3.519-0.322,5.186-0.977c3.637-1.438,6.527-4.318,7.97-7.956   L494.436,41.257C495.66,38.188,494.862,34.679,492.431,32.443z"/>
					</g>
				</svg>
			</button>
		</fieldset>
	</form>
</template>

<script setup lang="ts">
import { marked } from "marked";
import dompurify from "Dompurify";

const newMessage = ref("");
const messages = useMessages();
const { customerInitials } = useCustomer();
const isSubmitted = ref(false);

async function handleSubmit() {
	isSubmitted.value = true;

	messages.value.push({
		name: customerInitials.value,
		message: newMessage.value,
		isAI: false,
		timestamp: new Date().toLocaleString([], {
			timeStyle: "short",
		})
	});

	const userMessage = newMessage.value;
	newMessage.value = "";

	await $fetch("/api/message", {
		method: "POST",
		query: {
			message: userMessage,
		},
		async onResponse({response}){
			const content = response._data.data[0].content[0];

			if(content.type != "text"){
				return;
			}

			const parsedMessage = await marked.parse(
				dompurify.sanitize(content.text.value)
			);

			messages.value.push({
				name: "Bruno",
				message: parsedMessage,
				isAI: true,
				timestamp: new Date().toLocaleString([], {
					timeStyle: "short",
				}),
			});

			isSubmitted.value = false;
		}
	});
}
</script>