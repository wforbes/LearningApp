<template>
	<div id="math-page">
		<v-container pa-0 ma-0 pt-5>
			<v-row>
				<v-col class="pa-0 ma-0 ma-md-5" align-self="center">
					<v-tabs-items v-model="mainTab">
						<v-tab-item>
							<v-container pa-0 ma-0>
								<v-row>
									<v-col>
										<h1 style="text-align:center;">Math Practice</h1>
									</v-col>
								</v-row>
								<v-row>
									<v-col offset="1" cols="10" offset-md="2" md="10">
										<v-form class="mb-15">
											<v-row class="pa-0 ma-0">
												<v-col class="pa-0 ma-0 pr-3 pt-md-4" style="text-align:right" cols="5" md="5">
													<span>Amount of problems:</span>
												</v-col>
												<v-col class="pa-0 ma-0" cols="5" md="2">
													<v-text-field v-model="settings.numProblems" type="number" outlined></v-text-field>
												</v-col>
											</v-row>
											<div style="border: 0.1em solid grey; border-radius:0.2em; padding:0.3em;">
												<h4>Number Values</h4>
												<v-row class="pa-0 ma-0 mt-4" >
													<v-col class="pa-0 ma-0 pr-3" style="text-align:right" cols="5">
														<span>Highest number:</span>
													</v-col>
													<v-col class="pa-0 ma-0" cols="5">
														<v-text-field v-model="settings.maxNum" type="number" outlined></v-text-field>
													</v-col>
												</v-row>
												<v-row class="pa-0 ma-0 pt-2">
													<v-col class="pa-0 ma-0 pr-3" style="text-align:right" cols="5">
														<span>Lowest number:</span>
													</v-col>
													<v-col class="pa-0 ma-0" cols="5">
														<v-text-field v-model="settings.minNum" type="number" outlined></v-text-field>
													</v-col>
												</v-row>
												<v-row class="pa-0 ma-0">
													<v-col class="pa-0 ma-0 pt-3" style="text-align:right" cols="5">
														<span>Randomize:</span>
													</v-col>
													<v-col class="pa-0 ma-0" style="text-alight:center;" cols="5">
														<v-switch class="ma-0 pa-0 pt-3 pt-md-0 pl-7"
															v-model="settings.randomizeNums"
														></v-switch>
													</v-col>
												</v-row>
											</div>
											<div style="border: 0.1em solid grey; border-radius:0.2em; padding:0.3em; margin-top: 1em;">
												<h4>Problem Types:</h4>
												<v-row class="pa-0 ma-0">
													<v-col class="pa-0 pl-3 pr-3 ma-0" cols="12">
														<v-checkbox 
															v-for="(operation, index) in operationTypes"
															:key="index"
															v-model="settings.problemTypes" 
															:value="operation.symbol"
															:label="operation.name + ' (' + operation.symbol + ')'"
														></v-checkbox>
													</v-col>
												</v-row>
												<v-row>
													<v-col class="ma-0 pa-0 pt-3" style="text-align:right" cols="5">
														<span>Randomize:</span>
													</v-col>
													<v-col class="ma-0 pa-0" cols="3">
														<v-switch
															v-model="settings.randomizeTypes"
															class="ma-0 pa-0 pt-3 pt-md-0 pl-7"
														></v-switch>
													</v-col>
												</v-row>
											</div>
											<!-- TODO:
											<v-row>
												<v-col>
													<span>Time limit:</span>
												</v-col>
												<v-col>
													<v-text-field outlined></v-text-field>
												</v-col>
											</v-row>
											Increasing difficulty
											-->
											<v-row justify="space-around" align="center">
												<v-col cols="6" class="pa-0 ma-0 mt-3 pt-6" style="text-align:center;">
													<v-btn @click="begin()" color="success">Start</v-btn>
												</v-col>
											</v-row>
										</v-form>
									</v-col>
								</v-row>
							</v-container>
						</v-tab-item>
						<v-tab-item>
							<v-tabs v-model="problemTab" show-arrows>
								<v-tab v-for="(problem, index) in problems" :key="index">
									Problem {{ index+1 }}
								</v-tab>
							</v-tabs>
							<v-tabs-items v-model="problemTab">
								<v-tab-item v-for="(problem, index) in problems" :key="index">
									<!-- v-if="problem.type === 'vertical'" -->
									<v-container class="vertical-format-problem" style="width: 130px">
										<v-row>
											<v-col></v-col>
											<v-col style="text-align:right">
												<span class="display-2">
													{{ problem.num1 }}
												</span>
											</v-col>
										</v-row>
										<v-row>
											<v-col style="text-align:right">
												<span class="display-2">
													{{ problem.opSymbol }}
												</span>		
											</v-col>
											<v-col style="text-align:right">
												<span class="display-2">
													{{ problem.num2 }}
												</span>
											</v-col>
										</v-row>
										<v-row>
											<v-col>
												<hr />
											</v-col>
										</v-row>
										<v-row>
											<v-col>
												<v-text-field outlined v-model="problem.answer"></v-text-field>
											</v-col>
										</v-row>
										<v-row justify="space-around" align="center">
											<v-col cols="6" class="pa-0 ma-0 mt-3 pt-6" style="text-align:center;">
												<v-btn @click="next()" color="success">Next</v-btn>
											</v-col>
										</v-row>
									</v-container>
								</v-tab-item>
							</v-tabs-items>
						</v-tab-item>
					</v-tabs-items>
				</v-col>
			</v-row>
		</v-container>
	</div>
</template>

<script>
export default {
	name: "MathPage",
	data() {
		return {
			activeTab: 0,
			mainTab: 0,
			problemTab: 0,
			settings: {
				numProblems: 10,
				minNum: 0,
				maxNum: 10,
				problemTypes: [],
				randomizeTypes: true,
				randomizeNums: true
			},
			operationTypes: [
				{ 
					name: "addition",
					symbol:"+"
				}, 
				{ 
					name: "subtraction",
					symbol: "-" 
				},
				{ 
					name: "multiplication",
					symbol: "x" 
				}
			],
			problemFormats: ["vertical", "horizontal"],
			
			problems: [],
			currentProblem: {
				id: "",
				type: "",
				num1: 20,
				num2: 13,
				opSymbol: "x",
				answer: ""
			}
		};
	},
	methods: {
		begin() {
			this.generateProblemSet();
			this.currentProblem = this.problems[0];
			this.mainTab++;
		},
		next() {
			this.currentProblem = this.problems[this.activeTab+1];
			this.problemTab++;
		},
		generateProblemSet() {
			for(let i = 0; i < this.settings.numProblems; i++) {
				let n1 = Math.floor(Math.random() * this.settings.maxNum+1) + this.settings.minNum;
				let n2 = Math.floor(Math.random() * this.settings.maxNum+1) + this.settings.minNum;
				let typeIndex = Math.floor(Math.random() * this.settings.problemTypes.length);
				this.problems.push(this.createProblem(n1, n2, typeIndex));
			}
		},
		createProblem(n1, n2, typeIndex) {
			return {
				type: "vertical",
				num1: n1,
				num2: n2,
				opSymbol: this.operationTypes[typeIndex].symbol,
				answer: ""
			}
		}
	}
}
</script>