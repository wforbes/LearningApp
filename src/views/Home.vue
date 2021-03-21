<template>
	<div class="home">
		<v-container>
			<v-row>
				<v-col align-self="center" cols="6" offset-sm="3">
					<v-card class="pa-4">
						<h2>Welcome to LearningApp</h2>
					</v-card>
				</v-col>
			</v-row>
			<v-row>
				<v-col cols="3">
					<v-card>
						<v-card-title>Mathematics</v-card-title>
					</v-card>
				</v-col>
				<v-col cols="3">
					<v-card>
						<v-card-title>Programming</v-card-title>
					</v-card>
				</v-col>
				<v-col cols="3">
					<v-card>
						<v-card-title>Mathematics</v-card-title>
					</v-card>
				</v-col>
			</v-row>
		</v-container>
	</div>
</template>

<script>

export default {
	name: 'Home',
	components: {}
}
</script>
