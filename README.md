# Vue2.0-dialog-component

## Summary:

This is a dialog component, where you can show some html elements as dialog with your own style. 
It is simple and works like dialog did in JqueryUI dialog plugin.
You can define your own title, size, buttons. Also it is draggable.

## Property:

Title: String
Modal: Boolean
Width: Number
Height: Number
visible: Boolean
buttons: Object

## Template:

```
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
```

## Usage:

1. import the component from dialog.vue

```
  import VueDialog from './components/dialog'
```

2. define the dialog html in your component template

```
      <div>
          <button @click="visible = true">Click to display dialog</button>
          <vue-dialog :visible="visible" @close="close" :title="title" :buttons="buttons" :width="width">
            ...
          </vue-dialog>
        </div>
```

3. set your data to display the dialog. 

> Note: your should define close method to hide it by change visible to false.

```
export default {
  name: 'test',
  data () {
    return {
      msg: 'hello there',
      visible: false,
      sides: 80,
      title: 'test title',
      width: 500,
      buttons: {
        'Close': "cancelEvent",
        'OK': "confirmEvent"
      }
    }
  },
  methods: {
    confirmEvent() {
      console.log("Confirmed!");
      alert("Confirmed!");
      this.close();
    },
    cancelEvent(){
      console.log("Canceled!"); 
      alert("Canceled!"); 
      this.close();
    },
    close(){
      this.visible = false;
    }
  }
}
```

## DEMO Screen Shot 

![](){https://github.com/MichaelU3/Vue2.0-dialog-component/blob/master/demo.PNG}

