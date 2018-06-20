<template>
		<li class="task">
			<template v-if="!stateEdit">
				<span :class="task.stateComplete ? 'complete': 'incomplete'">{{ task.nameTask }}</span>
				<div class="icons-container">
					<span>
						<i class="material-icons" @click="changeStateComplete">
							{{ !task.stateComplete ? 'indeterminate_check_box' : 'check_box' }}
						</i>
					</span> 
					<span> <i class="material-icons" @click="edit">edit</i></span>
					<span> <i class="material-icons" @click="remove">delete</i></span>
				</div>
			</template>
			<template v-else>
				<input v-model="temp" type="text">
				<div class="icons-container">
					<span><i class="material-icons" @click="update">check</i></span>
					<span><i class="material-icons" @click="cancelEdit">cancel</i></span>  
				</div>
			</template>
		</li>
</template>
<script>
	import EventBus from './eventBus.js'
	export default {
		template : '#template-task',
		props: ['tasks','task', 'index'],
		data(){
			return{
				temp: '',
				stateEdit: false
			}
		},
		created(){
			EventBus.$on('editing', (index) => {
				if(this.index != index){
					this.cancelEdit();
				}
			});
		},
		methods: {
				changeStateComplete(){
					this.task.stateComplete = !this.task.stateComplete;
				},
				edit(){
					EventBus.$emit('editing', this.index);
		            this.stateEdit = true;
					this.temp = this.task.nameTask;
				},
				update(){
				 	if(this.temp != ''){
				 		this.task.nameTask = this.temp;
				 		this.cancelEdit();
				 		this.temp = '';
				 		return true	
				 	}
				 	return M.toast({html: 'No se permiten campos vacios', classes:'toast-down-right'});
				},
				cancelEdit(){
					this.stateEdit = false;
				},
				remove(){
					this.tasks.splice(this.index, 1);
				}
			}
		}
</script>
<style lang="scss">
	
	.task{
		align-items: center;
    	border-bottom: 2px solid white;
		display: flex;
		padding: .5em;
		.complete{
			text-decoration: line-through;
			color: #000;
		}
		.incomplete{
			text-decoration: none;
		}
		input[type="text"]{

		}
		.icons-container{
			display: flex;
			margin-left: auto;
			align-items: center;

			span{
				cursor: pointer;
				display: flex;
				justify-content: space-around;
				align-items:center;
			}
		}
	}

</style>