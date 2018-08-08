<template>
	<v-app dark>
		<v-toolbar color="cyan" dark floating>
			<v-toolbar-side-icon></v-toolbar-side-icon>
			<v-toolbar-title>Layout</v-toolbar-title>
			<v-btn outline><v-icon>search</v-icon></v-btn>
			<v-btn outline><v-icon>home</v-icon></v-btn>
			<v-btn outline><v-icon>event</v-icon></v-btn>
			<v-btn outline><v-icon>info</v-icon></v-btn>
			<v-btn right absolute outline>useless button</v-btn>
		</v-toolbar>
		<v-content>
			<v-container>
				<nuxt />
			</v-container>
		</v-content>
		<v-footer class="pa-3">
			<v-spacer></v-spacer>
				<div>{{ new Date() }}</div>
		</v-footer>
	</v-app>
</template>