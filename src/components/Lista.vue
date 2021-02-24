<template>
    <div class="container-fluid">
        <div class="row justify-content-center">
            <h1 class="col-12" style="text-align: center;">
                Ordena tus ingredientes.
            </h1>
        </div>
        <div class="row justify-content-center">
            <ul class="list-group drag col-6" id="comprados">
                <h4>Ingredientes en Existencia</h4>
                <li class="list-group-item">
                    <input placeholder="Agrega un nuevo ingrediente en existencia" type="text" class="new-todo todo-item w-100" v-model="newItem" @keyup.enter="addItemComprado">
                </li>
                <li class="list-group-item list-group-item-action" v-for="(ingrediente, i) in IngredientesComprados" :key="i"
                draggable="true" @dragstart="dragStart(i, $event, 0)" @dragover.prevent 
                @dragenter="dragEnter" @dragleave="dragLeave" @dragend="dragEnd" @drop="dragFinish(i, $event, 0)">
                    <span style="color: white;">{{ ingrediente }}</span>
                    <span class="remove-item" @click="removeItemAt(i, 0)">x</span>
                </li>
            </ul>
            <ul class="list-group drag col-6" id="porComprar">
                <h4>Ingredientes por Comprar</h4>
                <li class="list-group-item">
                    <input placeholder="Agrega un nuevo ingrediente por comprar" type="text" class="new-todo todo-item w-100" v-model="newItem2" @keyup.enter="addItemPorComprar">
                </li>
                <li class="list-group-item list-group-item-action" v-for="(ingrediente, i) in IngredientesPorComprar" :key="i" 
                draggable="true" @dragstart="dragStart(i, $event, 1)" @dragover.prevent 
                @dragenter="dragEnter" @dragleave="dragLeave" @dragend="dragEnd" @drop="dragFinish(i, $event, 1)">
                    <span style="color: white;">{{ ingrediente }}</span>
                    <span class="remove-item" @click="removeItemAt(i, 1)">x</span>
                </li>
            </ul>
        </div>
        <div class="row justify-content-center">
            <div class="dot-drop"  @dragover.prevent @drop="dragFinish(-1, $event, -1)">
                <span class='icon' v-if="dragging > -1"></span>
                <p v-else>Arrastra aquí para borrar</p>
            </div>
        </div>
        
    </div>
</template>

<script>
export default {
    data(){
        return{
            IngredientesComprados: ['Sal', 'Pollo', 'Tomates', 'Pimienta', 'Crema'],
            IngredientesPorComprar: ['Jamón', 'Piña', 'Harina', 'Maíz', 'Azucar'],
            newItem: "",
            newItem2: "",
            dragging: -1
        }
    },
    methods: {
        addItemComprado() {
            if (!this.newItem) {
                console.log('Void item');
                return;
            }
            this.IngredientesComprados.push(this.newItem);
            this.newItem = "";
        },
        addItemPorComprar() {
            if (!this.newItem2) {
                console.log('Void item');
                return;
            }
            this.IngredientesPorComprar.push(this.newItem2);
            this.newItem2 = "";
        },
        dragStart(which, ev, who) {
            ev.dataTransfer.setData('Text', this.id);
            ev.dataTransfer.dropEffect = 'move'
            this.dragging = which;
            this.origin = who;
            ev.target.style.backgroundColor = '#A3BCB6';
        },
        dragEnter(ev) {
            console.log(ev.clientY);
        },
        dragLeave(ev) {
            console.log(ev.target);
            
        },
        dragEnd(ev) {
            console.log(ev.target);
            this.dragging = -1;
            if(this.origin  == 0){
                ev.target.style.backgroundColor = '#3C403D';
            }else{
                ev.target.style.backgroundColor = '#39603D';
            }
            
        },
        dragFinish(to, ev, who) {
            if(to > -1){
                this.moveItem(this.dragging, to, this.origin, who);
            }else{
                this.removeItemAt(this.dragging, this.origin);
            }
        },
        //Los mismo eventos para que funcione en una pantalla touch
        //Ahora con touchstart, touchleave.. etc.
        moveItem(from, to, origin, destiny) {
            if(origin == destiny){
                if(origin == 0){
                    this.IngredientesComprados.splice(to, 0, this.IngredientesComprados.splice(from, 1)[0]);
                }else{
                    this.IngredientesPorComprar.splice(to, 0, this.IngredientesPorComprar.splice(from, 1)[0]);
                }
            }else{
                if(origin == 0 && destiny == 1){
                    this.IngredientesPorComprar.splice(to, 0, this.IngredientesComprados.splice(from, 1)[0]);
                }else{
                    this.IngredientesComprados.splice(to, 0, this.IngredientesPorComprar.splice(from, 1)[0]);
                }
            }
        },
        removeItem(item) {
            this.IngredientesComprados.splice(this.IngredientesComprados.indexOf(item), 1);
        },
        removeItem2(item) {
            this.IngredientesPorComprar.splice(this.IngredientesPorComprar.indexOf(item), 1);
        },
        removeItemAt(index, who) {
            if(who == 0){
                this.IngredientesComprados.splice(index, 1);
            }else{
                this.IngredientesPorComprar.splice(index, 1);
            }
        }
    }
}
</script>

<style scoped>
    .row{
        padding-top: 15px;
    }
    .remove-item {
        float: right;
        color: rgb(236, 177, 177);
        opacity: 0.5;
    }
    .dot-drop {
        margin-top: 10px;
        height: 150px;
        width: 150px;
        background-color: #A3BCB6;
        /*background-color: #a45;*/
        border-radius: 50%;
        display: inline-block;
        text-align: center;
        vertical-align: middle;
        font-size: 1.1em;
    }

    .icon {
        background: url('../icons/eliminar.png') no-repeat center;;
        height: 150px;
        width: 150px;
        border-radius: 50%;
        display: block;
        position: absolute;
        background-color: #a45;
        /* Other styles here */
    }

    .dot-drop p {
        padding-top: 50px;
    }

    .container-fluid{
        padding-left: 20px;
        padding-right: 20px;
    }

    ul{
        border-radius: 15px;
        min-height: 60vh;
        max-height: 60vh;
        border: 1px solid black;
        overflow: auto;
        padding-left: 10px;
    }
    
    li{
        border-radius: 15px;
        margin-top: 5px;
        margin-bottom: 5px;
        font-size: 1.1em;
    }

    input[type="text"]
    {
        background: transparent;
        border: none;
        color: #fff;
    }

    #porComprar li{
        background-color: #39603D;
    }

    #comprados li{
        background-color: #3C403D;
    }
</style>