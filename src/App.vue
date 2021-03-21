<template>
	<v-app>
		<v-app-bar
			app
			color="primary"
			dark
		>
			<div class="d-flex align-center">
				<h1>LearningApp</h1>
			</div>

			<v-spacer></v-spacer>

			<v-btn
				href="https://wforbes.net"
				target="_blank"
				text
			>
				<span class="mr-2">wforbes.net</span>
				<v-icon>mdi-open-in-new</v-icon>
			</v-btn>
		</v-app-bar>

		<v-main>
			<router-view></router-view>
		</v-main>
	</v-app>
</template>

<script>

export default {
  name: 'App',

  components: {},

  data: () => ({
    //
  }),
};
</script>
