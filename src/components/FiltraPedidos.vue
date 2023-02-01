<template>
    <div>
        <MessageComponent :msg="msg" v-show="msg"/>
        <div>
           <div id="pesquisa">
                <p>Filtre os Pedidos por Status:</p>
                <select name="status" class="status" v-model="status_selec" @change="filtraPedidos">
                    
                    <option v-for="s in status" :key="s.id" :value="s.tipo"> <!--o status do burger vai receber o tipo de status selecionado-->
                        {{ s.tipo }}
                    </option>
                </select>
                <button class="lista-btn" @click="todosPedidos()">Todos os Pedidos</button>
           </div>
            <div id="burger-table-heading">
                <div class="order-id">#:</div>
                <div>Cliente:</div>
                <div>Pão:</div>
                <div>Carne:</div>
                <div>Opcionais:</div>
                <div>Ações:</div>
            </div>
            <div id="burger-table-rows">
                <div class="burger-table-row" v-for="burger in burgers" :key="burger.id">
                    <div class="order-number">{{ burger.id }}</div>
                    <div>{{ burger.nome }}</div>
                    <div>{{ burger.pao }}</div>
                    <div>{{ burger.carne }}</div>
                    <div>
                        <ul>
                            <li v-for="(opcional, index) in burger.opcionais" :key="index" >
                                {{ opcional }}
                            </li>
                        </ul>
                    </div>
                    <div>
                        <select name="status" class="status" @change="updatedBurger($event, burger.id)">
                            <option value="">Selecione</option>
                            <option v-for="s in status" :key="s.id" :value="s.tipo" :selected="burger.status == s.tipo"> <!--o status do burger vai receber o tipo de status selecionado-->
                                {{ s.tipo }}
                            </option>
                        </select>
                        <button class="delete-btn" @click="deleteBurger(burger.id)">Cancelar</button>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import MessageComponent from './MessageComponent.vue';
export default {
    name: "FiltraPedidos",
    data() {
        return {
            burgers: [],
            burger_id: null,
            status: [],
            msg: null,

            data: [],
            status_selec: null,   
        };
    },
    components: {
        MessageComponent
    },
    methods: {
        async getPedidos() {
            const req = await fetch("http://localhost:3000/burgers");
            this.data = await req.json();

            // for (let index = 0; index < data.length; index++) {
            //     const element = data[index];
            //     if (element.status == "Em produção"){
            //         this.burgers.push(element);;
            //     }
            // }

            // data.forEach(element => {
            //     if (element.status == "Em produção"){
            //         this.burgers.push(element);;
            //     }
            // });

            // this.burgers = data.filter(element => {
            //     if (element.status == "Solicitado"){
            //         return element;
            //     }
            // }).map(element => {
            //     element.nome = element.nome+'_cge';
            //     return element
            // })


            this.burgers = this.data;
            console.log(this.burgers);
            //resgatar o status
            this.getStatus();
        },


        async getStatus() {
            const req = await fetch("http://localhost:3000/status");
            const data = await req.json();
            this.status = data;
            console.log(data);
        },
        async deleteBurger(id) {
            const req = await fetch(`http://localhost:3000/burgers/${id}`, {
                method: "DELETE"
            });
            const res = await req.json();

            //colocar uma msg no sistema
            this.msg = `Pedido removido com sucesso!`;
            //limpar msg
            setTimeout(() => this.msg = "", 3000);

            this.getPedidos();
        },
        async updatedBurger(event, id) {
            const option = event.target.value;
            const dataJson = JSON.stringify({ status: option });
            const req = await fetch(`http://localhost:3000/burgers/${id}`, {
                method: "PATCH",
                headers: { "Content-Type": "application/json" },
                body: dataJson
            });
            const res = await req.json();

            //colocar uma msg no sistema
            this.msg = `O pedido Nº ${res.id} está: *${res.status}*.`;
            //limpar msg
            setTimeout(() => this.msg = "", 500);

            console.log(res);
        },

        async filtraPedidos() {
            this.burgers = this.data.filter(element => {
                if (element.status == this.status_selec) {
                    return element;
                }            
            });  
        },

        todosPedidos() {
            this.getPedidos();
        }
        
    },
    mounted() {
        this.getPedidos();
    }
}
</script>

<style scoped>
    #burger-table {
        max-width: 1200px;
        margin: 0 auto;
    }
    #burger-table-heading,
    #burger-table-rows,
    .burger-table-row {
        display: flex;
        flex-wrap: wrap;
    }
    #burger-table-heading {
        font-weight: bold;
        padding: 12px;
        border-bottom: 3px solid #333;
    }
    #burger-table-heading div,
    .burger-table-row div {
        width: 19%;
    }
    .burger-table-row {
        width: 100%;
        padding: 12px;
        border-bottom: 1px solid #CCC;
    }
    #burger-table-heading .order-id, 
    .burger-table-row .order-number {
        width: 5%;
    }
    select {
        padding: 12px 6px;
        margin-right: 12px;
    }
    .delete-btn {
        background-color: #222;
        color: #FCBA03;
        font-weight: bold;
        border: 2px solid #222;
        padding: 10px;
        font-size: 16px;
        margin: o auto;
        cursor: pointer;
    }
    .delete-btn:hover {
        background-color: #DA291C;
        color: #222;
    }


    #pesquisa {
        display: flex;  
    }
    .lista-btn {
        background-color: #FCBA03;
        color: #222;
        font-weight: bold;
        border: 2px solid #222;
        padding: 10px;
        font-size: 16px;
        margin: o auto;
        cursor: pointer;
    }
    .lista-btn:hover {
        background-color: #FFF;
        color: #ebb31a;
        text-decoration: underline;
    }
</style>