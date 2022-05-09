<template>
	<view>
		<view class="uni-form-item uni-column">
		     <picker @change="examinationType" :range="examinationTypeArray">
		     <label class="">{{ examinationTypeArrayType }}</label>
		     </picker>
		</view>
		<tlbs-radio v-model="radio" label="1">备选项</tlbs-radio>
		  <tlbs-radio v-model="radio" label="2">备选项</tlbs-radio>
	</view>
</template>

<script>
	export default{
		onReady: function() {
		     this.$refs.form.setRules(this.rules);
		},
		data(){
			return{
				examinationTypeArray: ['请选择', '11111', '22222', '33333', '44444'],
				examinationTypeIndex: 0,
				examinationTypeArrayType: 'ccc',
				radio: '1'
			}
		},
		methods:{
			 examinationType(e) {
			     this.examinationTypeIndex = e.target.value;
			     this.examinationTypeArrayType = this.examinationTypeArray[this.examinationTypeIndex];
			 },
		}
	}
</script>

<style>
	
</style>