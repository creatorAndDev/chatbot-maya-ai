<template>
	<div class="bg-repeat bg-cover w-full h-full font-sans py-5">
		<div class="flex items-center justify-center h-full">
			<div class="max-w-[800px] relative z-20 after:content-[' '] after:z-[-1] after:h-full after:border-4 after:border-white/[.75] after:rounded-b-sm after:block after:w-11/12 after:m-auto">
				<div class="bg-white/[.90] overflow-hidden p-6 rounded">
					<!-- Start Screen -->
					<Start v-if="!isChatting" />

					<!-- Chat Screen -->
					<Chat v-else />
				</div>
			</div>
		</div>
	</div>
</template>

<script setup lang="ts">
const isChatting = useIsChatting();

// use Node.js v21
//
</script>

<style>
html{
	height:100%;
	min-width:320px;
	-webkit-text-size-adjust: 100%;
	-ms-text-size-adjust: 100%;
}
body{
	height: 100%;
	min-width:320px;
	position:relative;
	/* background: url('/bg.png') no-repeat; */
	background: #16202e;
}

#__nuxt{
	width: 100%;
	min-height: 100%;
	display: flex;
	align-items: center;
}
*{
	box-sizing: border-box;
}
textarea{
	resize: none;
	outline: none;
}
textarea:focus,
input:focus{
	outline: none;
}
button:disabled{
	opacity: .2;
}
</style>