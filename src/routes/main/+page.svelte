<script lang="ts">
    let player=true
    let valores: string[][] = [];
    let indexes: [number, number][] = [];

    function inicializar(n: number): void {
        valores = [];
        indexes = [];
        for (let i = 0; i < n; i++) {
            let linha: number[] = [];
            for (let j = 0; j < n; j++) {
                linha.push('');
                indexes.push([i, j]); // <- Pares de coordenadas
            }
            valores.push(linha);
        }
    }

    inicializar(3);

    function add(i: number, j: number): void {
        if (player){
            valores[i][j] += 'X';
        }
        else{
            valores[i][j] += 'O';
        }
    }
    function switch_player():void{
        player=!player
    }
    function reset():void{
        player=true
        valores= [];
        indexes= [];
        inicializar(3);
    }
    function velha():void{
        let cheio = false;
        valores.forEach(y => {
            y.forEach(Element=>{
                if(!Element){
                    cheio=true
                }  
            })
        });
        if(!cheio){
            reset()
        }
    }
    function coluna():boolean{
        let retorno=false
        for (let i=0;i<valores.length;i++){
            if(valores[0][i]==valores[1][i] && valores[2][i]==valores[0][i] && valores[0][i]){
                retorno = true
            }          
        }
        return retorno
    }
    function linha():boolean{
        let retorno=false
        valores.forEach(y => {
            if(y[0]==y[1] && y[0]==y[2] && y[0]){
                retorno= true
            }
        })
        return retorno
    }
    function diagonais():boolean{
        if(valores[0][0]==valores[1][1] && valores[0][0]==valores[2][2] && valores[0][0]){
            return true;
        }
        if(valores[0][3]==valores[1][1] && valores[0][3]==valores[2][0] && valores[0][0]){
            return true;
        }
        return false
    }
    function vitoria(){
        if(coluna()||linha()||diagonais()){
            reset()
        }
    }
    function inserir(i:number,j:number):void{
        add(i,j);
        switch_player();
        vitoria()
        velha()
    }
    function jogada(i:number,j:number):void{
        if(!valores[i][j]){
            inserir(i,j)
        }
        else{
            alert('calma lá paizão')
        }
    }
</script>

<!-- Agora desestruturando os índices diretamente -->
  <div class="flex justify-center items-center h-screen">
    <div class="grid grid-cols-3 w-[45rem] h-[45rem] max-w-full max-h-full gap-0">
      {#each indexes as [i, j]}
        <button 
        on:click={() => jogada(i, j)} 
        class="w-[15rem] h-[15rem] rounded-none text-gray-900 bg-white border border-gray-300 focus:outline-none focus:ring-0 active:transform-none hover:bg-gray-100 font-medium text-sm dark:bg-gray-800 dark:text-white dark:border-gray-600 dark:hover:bg-gray-700 dark:hover:border-gray-600"
        >
            {valores[i][j]}
        </button>
      {/each}
    </div>
  </div>