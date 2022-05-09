<template>
	<view>
		<view class="content">
					<view class="maintenanceContent1" v-for="(item) in arr1">
						<view class="first1">
							<view>
								{{item.systemName}}
							</view>
							<view class="second" v-for="(item2) in item.subsystems">
								<view>
									{{item2.subsystemName}}
								</view>
								<view class="third">
									<label v-for="(item3, index) in item2.facilities" :key="item3.subplanId">
										<checkbox-group name="" @change="checkboxChange($event, item3.subplanId)" >
											<view><checkbox  :checked="checkAll"  :value="item3.subplanId"/> 
											<text>{{item3.facilitiesName}}</text></view>
										</checkbox-group>
									</label>
								</view>
								
							</view>
						</view>
						
					</view>
		</view>
		<view class="btn" scroll-y="false">
			<button  type="default" @click="submit">自建测试任务</button>
		</view>	
	</view>
</template>

<script>
	export default {
		data() {
			return {
				
			}
		},
		methods: {
			
		}
	}
</script>

<style>
	.maintenanceContent1{
			   border: 1px solid #CDC9C9;
			   margin:10px;
			   border-radius: 5px;
			   background-color: white;
	}
	.first1{
			   border-left: 3px solid #FF4500;
			   margin-left:10px;
	}
	.maintenanceContent1 view{
			   margin:10px;
			   font-size: 13px;
			   
	}
	
	button {
		   background-color: #FF6347;
		   color: white;
		   text-align:center;
		   height: 40px;
		   width: 320px;
		   border-radius:10px;
		   position: absolute;
		   right: 5px;
		   bottom: 60px;
		   left: 5px;
		   
	}

</style>
