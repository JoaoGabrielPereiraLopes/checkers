<script lang="ts">
    import pecabranca from '../assets/pecabranco.png';
    import pecapreta from '../assets/pecapreto.png';
    let click=false;
    let player=true;
    let valores: string[][] = [];
    let indexes: [number, number][] = [];
    let movivel: boolean[][] = [];
    function inicializar(n: number): void {
        valores = [];
        indexes = [];
        for (let i = 0; i < n; i++) {
            let linha: string[] = [];
            let booleanlinha: boolean[] = [];
            
            for (let j = 0; j < n; j++) {
                linha.push('');
                booleanlinha.push(false);
                indexes.push([i, j]);
            }
            valores.push(linha);
            movivel.push(booleanlinha)
        }
    }

    function posicionarBranco():void{
        for(let i=0;i<=2;i++){
            for(let j=0;j<=7;j++){
                if((i+j)%2){
                    valores[i][j]="PB"
                }
            }
        }
    }
    function posicionarPreto():void{
        for(let i=7;i>=5;i--){
            for(let j=0;j<=7;j++){
                if((i+j)%2){
                    valores[i][j]="PP"
                }
            }
        }
    }
    function switch_player():void{
        player=!player
    }
    function reset():void{
        player=true
        valores= [];
        indexes= [];
        inicializar(8);
        posicionarBranco()
        posicionarPreto()
    }
    reset()
    function jogavel(i:number,j:number,color:boolean,dama:boolean):void{
        if(color && !dama && player){
            if(i+1<=7){
                if(j-1>=0){
                    movivel[i+1][j-1]=true
                }
                if(j+1<=7){
                    movivel[i+1][j+1]=true
                }
            }
        }
        if(!color && !dama && !player){
            if(i-1>=0){
                if(j+1<=7){
                    movivel[i-1][j+1]=true
                }
                if(j-1>=0){
                    movivel[i-1][j-1]=true
                }
            }
        }
    }

</script>

<!-- Agora desestruturando os índices diretamente -->
<div class="flex justify-center items-center h-screen">
    <div class="grid grid-cols-8 w-[40rem] h-[40rem] max-w-full max-h-full gap-0">
      {#each indexes as [i, j]}
        <button  
          class=" {
            player ?
            valores[i][j] === 'PB'?"player":"":
            valores[i][j] === 'PP'?"player":""
          } w-[5rem] h-[5rem] rounded-none text-gray-900 {movivel[i][j] ? 'bg-lime-500' : (i+j)%2 ? 'bg-black' : 'bg-white'} focus:outline-none focus:ring-0 active:transform-none font-medium"
          on:mouseover={() => {
            if (valores[i][j] === 'PB') {
              jogavel(i, j, true, false);
            } else if (valores[i][j] === 'PP') {
              jogavel(i, j, false, false);
            }
            else{
                ()=>{}
            }
          }}
        >
        {#if valores[i][j]=='PB'}
            <img src={pecabranca} alt="" class="h-[4rem] w-[4rem] mx-auto" >
        {:else if valores[i][j]=='PP'}
            <img src={pecapreta} alt="" class="h-[4rem] w-[4rem] mx-auto" >
        {/if}
        </button>
      {/each}
    </div>
  </div>

  <style>
    .player:hover {
    cursor: pointer;
}
  </style>