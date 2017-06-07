<template>
	 <div v-show="visible" class="vue-dialog">
	  	<div class="modal" v-show="modal"></div>
		<div class="Vdialog" ref="dlgWindow">
			<div class="title" @mousedown="drag($event)" ref="dlgTitle">
				<div class="name">{{ title }}</div>
				<div @click="closeBtnEvt()" class="close"></div>
			</div>
			<div class="main" :style="{width: width + 'px', height: height + 'px'}" ref="dlgMain">
				 <slot></slot>
			</div>
			<div v-show="hasButton" class="buttons">
				<div v-for="(value, key) in buttons" @click="close(value)" class="button">{{ key }}</div>
			</div>
		</div>
	</div>
</template>

<script>
export default {
	methods:{
		drag(e) {
			let dragTitle = this.$refs['dlgTitle'];
			let moveBox = this.$refs['dlgWindow'];

			var disX=e.clientX-moveBox.offsetLeft;
			var disY=e.clientY-moveBox.offsetTop;
			document.onmousemove = (e) =>{
				e.preventDefault();
				var l=e.clientX-disX;
				var t=e.clientY-disY;
				var x = document.documentElement.clientWidth-moveBox.offsetWidth;
				var y = document.documentElement.clientHeight-moveBox.offsetHeight;
				l = l < 0 ? 0 : (l > x ? x : l);
				t = t < 0 ? 0 : (t > y ? y : t);
				moveBox.style.left=l+'px';
				moveBox.style.top=t+'px';
				return false;
			}
			document.onmouseup = () => {
				document.onmousemove = null;    
				document.onmouseup = null;
				return false;
			}
			return false;
		},
		autosize(){
			this.$nextTick( () => {
				var dom = this.$refs['dlgWindow'];
		        var CHeight = document.documentElement.clientHeight;
		        var CWidth = document.documentElement.clientWidth;
		        var PHeight = dom.offsetHeight;
		        var PWidth = dom.offsetWidth;
		        this.$refs['dlgMain'].style.maxHeight = CHeight - 100 +'px';
		        dom.style.top = (CHeight>PHeight?(CHeight-PHeight)/2:0) +'px';
		        dom.style.left = (CWidth-PWidth)/2 +'px';
			})
        },
        close(action){
			typeof(action) === "function" ? action() : this.$parent[action]();
		},
		closeBtnEvt(){
			this.$emit('close');
		}
    },
	props: {
		title: {
			type: String,
			default: "Display your title here!"
		},
		modal: {
			type: Boolean,
			default: true
		},
		width: {
			type: Number,
			default: 500
		},
		height: {
			type: Number,
			default: 400
		},
		visible:{
			type:Boolean,
			default:false
		},
		buttons:{
			type:Object,
			default(){
				return {}
			}
		}
	},
	computed: {
		hasButton(){
			return Object.keys(this.buttons).length > 0;
		}
	},
	watch: {
		visible (val) {
			val && this.autosize();
		}
	}
}
</script>

<style scoped>
    .vue-dialog .modal{position:fixed;left:0;top:0;background:rgba(0,0,0,0.3);width:100%;height:100%;z-index:100;}
	.vue-dialog .Vdialog{min-width:300px;background:#fff;z-index:101;position:fixed;left:50%;top:50%;box-shadow:0 0 10px #888;border-radius:5px;overflow:hidden;}
	.vue-dialog .Vdialog>.title{width:100%;height:50px;line-height:50px;text-align:left;font-size:16px;cursor:move;position:relative;background-color: #07c}
    .vue-dialog .Vdialog>.title>.name{padding-left:20px;font-weight:bold;}
	.vue-dialog .Vdialog>.title>.close{width:30px;position:absolute;right:0;top:0;cursor:pointer;font-size:30px;}
	.vue-dialog .Vdialog>.title>.close:after{content:'×';display:block;}
	.vue-dialog .Vdialog>.title>.close:hover{color:#fff;}
	.vue-dialog .Vdialog>.main{overflow-y:auto;overflow-x:hidden;}
	.vue-dialog .Vdialog>.buttons{display:flex;justify-content:flex-end;padding:10px 10px;box-shadow:0 0 6px #ccc;background-color: #3af}
	.vue-dialog .Vdialog>.buttons .button{flex:1;max-width:80px;text-align:center;cursor:pointer;height:36px;line-height:36px;box-shadow:0 0 1px #ccc;margin:0 10px;border-radius:5px;background-color:#BCBCBC;}
</style>