<template>
		<li class="task">
			<template v-if="!stateEdit">
				<span :class="task.stateComplete ? 'complete': 'incomplete'">{{ task.nameTask }}</span>
				<div class="icons-container">
					<a>
						<i class="material-icons" @click="changeStateComplete">
							{{ !task.stateComplete ? 'indeterminate_check_box' : 'check_box' }}
						</i>
						<span :class="!task.stateComplete ? 'complete-manual-tooltip' : 'back-manual-tooltip'"></span>
					</a> 
					<a> 
						<i class="material-icons" @click="edit">edit</i>
						<span class="edit-manual-tooltip"></span>
					</a>
					<a> 
						<i class="material-icons" @click="remove">delete</i>
						<span class="delete-manual-tooltip"></span>
					</a>
				</div>
			</template>
			<template v-else>
				<input v-model="temp" type="text">
				<div class="icons-container">
					<a>
						<i class="material-icons" @click="update">check</i>
						<span class="update-manual-tooltip"></span>
					</a>
					<a>
						<i class="material-icons" @click="cancelEdit">cancel</i>
						<span class="cancel-manual-tooltip"></span>
					</a>  
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
				 	return this.$emit('error', 'El campo no puede estar vacio');//M.toast({html: 'No se permiten campos vacios', classes:'toast-down-right'});
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
	//Variables
	$mapIcons:(
		complete: "Completar",
		back: "Reanudar",
		edit: "Editar",
		delete: "Eliminar",
		cancel: "Cancelar",
		update: "Actualizar"

	);

	//Mixins

		@mixin TooltipBody{
			position: absolute;
			bottom: -90%;
			background-color: #000;
			color: white;
			font-size: .9rem;
			padding: .2em;	
		}

		@mixin manual-tooltips($icons...){
			@for $i from 1 through length($icons){

				.#{nth($icons, $i)}-manual-tooltip{
					display: none;
					transition: opacity .5s;
					opacity: 0;	
					&::after{
						content: "#{map-get($mapIcons,nth($icons, $i))}";
					}
					@include TooltipBody;

					@media(max-width: 1080px){
						display: none !important;
					}
				}
			}
			

		}

	//End Mixins


	a{
		text-decoration: none;
	}
	.task{
		align-items: center;
    	border-bottom: 2px solid white;
		display: flex;
		padding: .5em;
		word-break: break-word;
		.complete{
			text-decoration: line-through;
			color: #000;
		}
		.incomplete{
			text-decoration: none;
		}
		.icons-container{
			display: flex;
			margin-left: auto;
			align-items: center;

			a{
				cursor: pointer;
				display: flex;
				justify-content: space-around;
				align-items:center;
				position: relative;
				@include manual-tooltips(map-keys($mapIcons)...);
				
				&:hover [class*="-manual-tooltip"]{
					display: block;
					opacity: 1;
					
				}
				
			}

		}
	}
</style>