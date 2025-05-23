<script lang="ts" src="">
    import pecabranca from '../assets/pecabranco.png';
    import pecapreta from '../assets/pecapreto.png';
    let iselect:number|null=null
    let jselect:number|null=null
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
                if(j-1>=0 && !valores[i+1][j-1]){
                    movivel[i+1][j-1]=true
                }
                if(j+1<=7 && !valores[i+1][j+1]){
                    movivel[i+1][j+1]=true
                }
            }
        }
        if(!color && !dama && !player){
            if(i-1>=0){
                if(j+1<=7 && !valores[i-1][j+1]){
                    movivel[i-1][j+1]=true
                }
                if(j-1>=0 && !valores[i-1][j-1]){
                    movivel[i-1][j-1]=true
                }
            }
        }
    }
    function desjogavel():void{
        if(!click){
            movivel=[]
            for (let i = 0; i < valores.length; i++) {
                let booleanlinha: boolean[] = [];
                
                for (let j = 0; j < valores.length; j++) {
                    booleanlinha.push(false);
                }
                movivel.push(booleanlinha)
            }
        }
    }
    function fixajogavel(i:number,j:number,color:boolean,dama:boolean){
        click=false
        desjogavel()
        click=true
        jogavel(i,j,color,dama)
        iselect=i
        jselect=j
    }
    function jogada(ifinal:number,jfinal:number):void{
        click=false
        desjogavel()
        valores[ifinal][jfinal]=valores[iselect][jselect]
        valores[iselect][jselect]=''
        switch_player()
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
            }
            {movivel[i][j]?"jogavel":""} w-[5rem] h-[5rem] rounded-none text-gray-900 {movivel[i][j] ? 'bg-lime-500' : (i+j)%2 ? 'bg-black' : 'bg-white'} focus:outline-none focus:ring-0 active:transform-none font-medium"
            on:mouseover={() => {
                if (valores[i][j] === 'PB' && !click) {
                jogavel(i, j, true, false);
                } else if (valores[i][j] === 'PP' && !click) {
                jogavel(i, j, false, false);
                }
                else{
                    ()=>{}
                }
            }}
            on:mouseleave={()=>{
                    if (valores[i][j]) {
                        desjogavel();
                    }
                }
            }
            on:click={() => {
                if (valores[i][j] === 'PB') {
                    fixajogavel(i, j, true, false);
                } else if (valores[i][j] === 'PP') {
                    fixajogavel(i, j, false, false);
                }
                else if(movivel[i][j]){
                    jogada(i,j);
                }
                else{
                    ()=>{}
                }
            }}>
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
    .player:hover, .jogavel:hover {
        cursor: pointer;
    }
  </style>