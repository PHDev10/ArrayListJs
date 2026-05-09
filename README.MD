#Explicando o funcionamento dos métodos **map**, **filter** e **reduce** em **Javascript**

##Autor

Feito por **Pedro Henrique Santos de Pontes**

Os três são métodos do Javascript utilizados para manipulação de dados em arrays (listas) no Javascript. Cada um, apesar de possuírem uma "lógica bruta" semelhante - devido a serem utilizados para substituir laços for padrões com inúmeras funcionalidades e de grande escopo -, cada um trabalha de uma forma específica.
Para compreender como `.map`, `.filter` e `.reduce` funcionam, precisa-se entender o callback.
Callback é uma função passada para outra função como argumento. Isso é feito para que a função passada como argumento seja executada apenas posteriormente, sendo também por isso que a função é passada sem parênteses - para que não seja executada no momento da passagem -, onde o seu retorno é que o foco principal, pois será utilizado pela função a qual o callback foi passado como parâmetro.

Sintaxes:

```
Function Declaration:
function saudar() { console.log("Olá!"); }
metodo(saudar);

Function Expression:
const variavel = metodo(function() {
    console.log("Olá!");
});

Arrow Function:
metodo(() => console.log("Olá com arrow function!"));
```

##Método **map**##

Sintaxe:

```
const arrayMapeada = arrayOriginal.map(callback);
```

Por "trás dos panos", o `map` funciona como um laço for, com a principal diferença sendo o retorno dele: Cada elemento que estava na array original será retornando e inserido na nova array, cuja qual está recebendo o retorno de cada "loop" realizado pelo `map`, na mesma posição que o valor estava na array original.

Exemplo de uso:

```
const notasAlunosTurmaA = [
    [56, 98, 25],
    [78, 94],
    [89, 87, 67],
    [90, 87],
    [70, 64, 94]
];

const mediasAlunosTurmaA = notasAlunosTurmaA.map((notas => {
    let notasSomadas = 0;
    for (nota of notas) {
        notasSomadas += nota;
    }

    return (notasSomadas / 3).toFixed(1); //O toFixed(1) é para que os resultados tenham um mesmo "padrão"
}));

//Resultado esperado: [ '59.7', '57.3', '81.0', '59.0', '76.0' ]
```

Cada valor foi inserido na mesma posição que o valor na array original. Não é apenas o valor, é a organização dos valores na nova array. E não se pode ser esquecido um detalhe crucial: Apesar de *percorrer* a array, o `map` não faz qualquer alteração na mesma; o `map` apenas copia os dados, os retorna para a mesma posição que estavam na array original e termina seu serviço. Afinal, a array original é imutável.
Como cada elemento é inserido na mesma posição que estava na array original e cada um é percorrido apenas uma vez, a nova array terá o mesmo tamanho da array original. Assim, parecerá uma literal cópia da array original.
Mas o `map` não cria apenas "cópias de array". Ele permite criar arrays novas com o mesmo tamanho da array original e dar novas "funções" ou "utilizações" para cada valor.

Utilizando o `map`, o código fica muito mais enxuto, se comparado ao uso sucessivo do `for`, e o resultado pretendido - médias dos alunos - é alcançado, além de conseguir fazer isso independetemente do número de dados que haja dentro das arrays com as notas. O resultado de cada média ficará na mesma posição onde a array com as notas utilizadas para obter a média estava, garantindo maior organização.

##Método **filter**##

Sintaxe:

```
const arrayFiltrada = arrayOriginal.filter((elemento, indice, arrayOriginal) => {return //Aqui é onde fica a condição para que o valor seja retornado});
```

Basicamente, assim como um laço de repetição, o método `filter` percorre toda a array a qual é referenciado. A partir disso, ele gera uma nova array com os elementos que passam por uma condicional definida no return da função. Logo, diferente do `map`, a array gerada não precisa ter, necessariamente, o mesmo tamanho da array original.
Seguindo a sintaxe como base, dá para perceber que há mais de um parâmetro sendo passado para o `filter`. Deles, apenas o `elemento` é obrigatório, pois se trata do elemento que está sendo retornado da array original naquele momento; `indice` e `array` são opcionais, sendo responsável pela informação da posição do valor na array original e guardar uma cópia array original - com os mesmos valores e suas posições e tamanho -, respectivamente. Estes dois últimos parâmetros são utilizados para casos mais específicos, se comparados ao `elemento`.

Exemplo de uso:

```
const clientesPadroesBanco = [
    {
        "nome" : "Pedro",
        "saldo" : 100
    },
    {
        "nome" : "Letícia",
        "saldo" : 549
    },
    {
        "nome" : "João",
        "saldo" : 1000
    },
    {
        "nome" : "Maria",
        "saldo" : 1800
    },
    {
        "nome" : "Carlos",
        "saldo" : 700
    }
]

const clientesVipsBanco = clientesPadroesBanco.filter((cliente) => {
    if (cliente.saldo >= 1000) { return cliente; }
});

//Resultado esperado: [ { nome: 'João', saldo: 1000 }, { nome: 'Maria', saldo: 1800 } ]
```

Com o uso do `filter`, uma array teve seus dados filtrados e salvos em uma nova lista. Apesar de toda a array ter sido percorrida, a nova array gerada não possui o mesmo tamanho, dando ainda mais ênfase no nome do método - "filtrar" -, além de manter a array original inalterável.
Nesse caso de uso, foram inseridos na nova lista apenas os "clientes" que possuíam um saldo maior ou igual a 1000. Dessa forma, o reduce permaneceu com o padrão da array original - salvando os dados como objetos conforme confirmava a condição. Em outro caso, se eu quisesse filtrar de outra forma, poderia retornar apenas o campo "nome" de cada objeto.
O `filter` não modificou a lista original; ele extraiu os valores que eram pertinentes e os salvou com a mesma formatação e em ordem, pois pode-se ver que, na array original, o cliente de nome "João" estava antes do que o cliente de nome "Maria.

##Método **reduce**##

Sintaxe:

```
const arrayReduzida = arrayOriginal.reduce((acumulador, elemento, indice, arrayOriginal) => {}, valorInicial);
```

O objetivo do `reduce` é "unir" os dados da array selecionada. Ele vai percorrer cada valor e vai uni-lo aos demais, incrementando-o até que todos os valores estejam lidos e, consequentemente, unificados - reduzidos, se trazido para o contexto da nomenclatura. A utilização deste método é mais voltado para operações matemáticas ou contabilizações em larga escala em grandes concentrações de dados.
Tendo a sintaxe como base, percebemos que o callback possui os seguintes parâmetros: acumulador, elemento, indice, arrayOriginal e valorOriginal. Cada um possui uma utilidade.
O `acumulador` serve para guardar o incremento conforme o loop vai sendo realizado após a leitura de cada elemento da array; seu uso é obrigatório, pois necessitamos dele para obter o resultado final.
O `elemento` é o dado que está sendo lido durante aquele ciclo (loop), fazendo uma cópia da informação original para que a mesma seja lida pelo "acumulador"; seu uso é obrigatório, pois é essencial guardar a informação para que o primeiro parâmetro possa fazer o incremento.
O `indice` é a posição da informação na array original; seu uso não é obrigatório.
O `arrayOriginal` guarda uma cópia de toda a array original que está sendo lida pelo `reduce`; seu uso não é obrigatório.
O `valorInicial` guarda o valor que o acumulador irá possuir no ínicio da utilização do `reduce`, passando seu valor para o mesmo e o definindo; seu uso não é obrigatório, mas é recomendado - isso é, principalmente, para evitar o erro de "array vazia", caso nenhum dado seja retornado e incrementado ao `acumulador`, e que o valor inicial do `acumulador` seja o primeiro elemento da array original, o qual será utilizado como "base de início" para as demais incrementações, caso estas ocorram.

Exemplo de uso:

```
const disciplinasMinistradasNoIfpb = [
    {
        "disciplina" : "Matemática Voltada à Finanças",
        "curso" : "Gestão Comercial"
    },
    {
        "disciplina" : "Desenvolvimento Web I",
        "curso" : "TSI"
    },
    {
        "disciplina" : "Banco de Dados II",
        "curso" : "TSI"
    },
    {
        "disciplina" : "Segurança da Informação",
        "curso" : "TSI"
    },
    {
        "disciplina" : "APS",
        "curso" : "TSI"
    },
    {
        "disciplina" : "Bilogia",
        "curso" : "ensinoMedio"
    },
    {
        "disciplina" : "Direito",
        "curso" : "TSI"
    },
    {
        "disciplina" : "Práticas Currículares I",
        "curso" : "TSI"
    },
    {
        "disciplina" : "Sociologia",
        "curso" : "ensinoMedio"
    },
    {
        "disciplina" : "Metodologia e Meio Ambiente",
        "curso" : "TSI"
    }
];

const contabilizacaoDeDisciplinasDoCursoTsi = disciplinasMinistradasNoIfpb.reduce((acumulador, elemento) => {
    if (elemento.curso == "TSI") {
        return acumulador + 1;
    };
    return acumulador;
}, 0);
```

Através do `reduce`, um novo array será gerado portando apenas um único valor, pois foi o que foi definido como valor inicial para o `acumulador` - o número 0. De forma sucinta, é feita a conferência de informações sobre as disciplinas, vendo quais pertencem ao curso de TSI. Quando a condição é verdadeira, é incrementado "+ 1" no `acumulador`. Quando a condição é falsa, no caso deste exemplo de uso, tem de ser dado um `return` do `acumulador` sem quaisquer alterações feitas porque, caso isso não seja feito, a condição será dada como falsa e nada será retornado, gerando `undefined` e quebrando na próxima vez que a condição for verdadeira e a incrementação for feita.
