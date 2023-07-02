# Instalação do Typescript

## Instalando o Typescript

### Para instalar a biblioteca:

```
npm init -y
npm i typescript -D
```

### Se eu quiser compilar:

```
npx tsc index.ts
```
* tsc significa Typescript Compiler

 Isso vai gerar para mim um arquivo index.js (extensão .js) do index.ts, já compilado para Typescript


### Para executar o código index.ts:

```
npm i ts-node -D
```

Para executar o ts-node usando code-runner, podemos adicionar uma configuração - só para esse projeto - do VS Code para executar Typescript com Code Runner usando ts-node.

Basta criar uma pasta chamada **.vscode** com um arquivo **settings.json**.
Em seguida, basta abrir um JSON (abrindo chaves) e começar a escrever **code-runner**.
Uma configuração aparecerá. Basta clicar em **code-runner.executorMap**

Basta deixar o seguinte código:

```
{
    "code-runner.executorMap": {
        "typescript": "npx ts-node --files --transpile-only",
    }
}

```

## Instalando o ESLINT

### Para instalar o eslint

```
npm i eslint -D
npm i @typescript-eslint/eslint-plugin @typescript-eslint/parser -D
```

### Adicionar a configuração do eslint

Criar um arquivo na raíz chamado ***.eslintrc.js*** e adicionar a seguinte configuração:

```
module.exports = {
    env: {
        browser: true,
        es6: true,
        node: true,
    },
    extends: [
        'eslint:recommended',
        'plugin:@typescript-eslint/eslint-recommended',
        'plugin:@typescript-eslint/recommended',
    ],
    globals: {
        Atomics: 'readonly',
        SharedArrayBuffer: 'readonly',
    },
    parser: '@typescript-eslint/parser',
    parserOptions: {
        ecmaVersion: 11,
        sourceType: 'module',
    },
    plugins: ['@typescript-eslint'],
    rules: {},
};

```

Adicionar no **settings.json** (na pasta .vscode) a seguinte configuração para aplicar correções do ESLint automaticamente:

```
"editor.codeActionsOnSave": {
        "source.fixAll": true,
        "source.fixAll.eslint": true
    }
```

## Instalando o Prettier

### Para instalar o prettier

```
npm i prettier eslint-config-prettier eslint-plugin-prettier -D
```

Adicionar a seguinte linha no parâmetro **extends** do *.eslintrc.js*:

```
'plugin:prettier/recommended',

Ex:

extends: [
        'eslint:recommended',
        'plugin:@typescript-eslint/eslint-recommended',
        'plugin:@typescript-eslint/recommended',
        'plugin:prettier/recommended',
    ],
```

### Adicionar a configuração do Prettier

Criar um arquivo .prettierrc.js com o seguinte conteúdo:

```
module.exports = {
    semi: true,
    trailingComma: 'all',
    singleQuote: true,
    printWidth: 120,
    tabWidth: 4,
}
```

## Configurando o **tsconfig.json**

### Criar o arquivo

```
npx tsc --init
```

* o tsconfig.json é usado pelo tsc (Typescript Compiler) para compilar o código para Javascript

Dentro do array do parâmetro **lib**, configurar assim:

```
"lib": ["ESNext", "DOM"],
```
Em **outDir**, configurar assim:

```
"outDir": "./dist",
```

### Configurar para compilar o que estiver em src:

1. Fora do compilerOptions, criar outro parâmetro chamado **include** e adicionar o seguinte código:

```
"include": [
    "./src"
  ]

Ex:
{
  "compilerOptions": { ...
  },
  "include": [
    "./src"
  ]
}
```

2. Em seguida, criar uma pasta src com **index.ts** dentro.

3. Por último, basta executar o seguinte comando para compilar:

```
npx tsc
```

## Configurar o editorconfig

1. Instalar a extensão **EditorConfig for VS Code**

2. Clicar com o botão direito e gerar um editorconfig

3. Configurar as linhas finais para:

```
trim_trailing_whitespace = true
insert_final_newline = true
end_of_line = lf


Ex:

# EditorConfig is awesome: https://EditorConfig.org

# top-most EditorConfig file
root = true

[*]
indent_style = space
indent_size = 4
end_of_line = lf
charset = utf-8
trim_trailing_whitespace = true
insert_final_newline = true
end_of_line = lf
```



# Type Annotations

## Tipos Básicos

    1 - string
    2 - number
    3 - boolean
    4 - symbol
    5 - bigint

## Arrays

    Array<T> - é um genérico de array.

    Ex.:
        arrayDeNumeros: Array<number>
        arrayDeStrings: Array<string>

    Existe um outro tipo de representação

    Ex.:
        arrayDeNumeros: number[]
        arrayDeStrings: string[]

## Objetos

    let pessoa: { nome: string, idade: number, adulto?: boolean } = { 'Marcelo', 25, true }

    * Depois que você define um objeto e coloca os dois pontos (:), prestar atenção que não está definindo valores, e sim tipos

## Funções

    function soma(x: number, y: number): number {
        return x + y;
    }

    * Também há uma outra notação, que é a de arrow:

    const soma2: (x: number, y:number) => number = (x, y) => x + y;


# Tipo Any

    É um tipo definido implicitamente quando não definimos o tipo

    * Utilizar apenas em último caso


## Strict: true

    Há um item no tsconfig.json que se chama strict

    Se ele estiver como true, ativará todo o bloco abaixo dele
    Um dos itens desse bloco é o **noImplicityAny**, que não permite que o código tenha um any definido implicitamente
    Dará erro de compilação, a menos que tenha o **--transpile_only** definido na configuração do Code Runner


# Tipo void

    Usado em funções que não retornam nada


# Tipo object

    const objetoA: {
        chaveA: string;  // chave
        readonly chaveB: string;  // não posso alterar a chave
        chaveC?: string;  // posso não preencher a chave
        [key: string]: unknown;   // posso definir a chave na hora
    } = {
        chaveA: 'Valor A',
        chaveB: 'Valor B',
    };

    objetoA.chaveA = 'Outro Valor';
    objetoA.chaveC = 'Nova chave';
    objetoA.chaveD = 'Nova chave';


# Tipo Tuple

    É como um array com os tipos definidos

    const dadosCliente1 = [number, string] = [1, 'Luiz'];
    const dadosCliente2 = [number, string, string] = [1, 'Marcelo', 'Valois'];
    const dadosCliente3 = [number, string, string?] = [1, 'Marcelo'];
    const dadosCliente4 = [number, string, ...string[]] = [1, 'Marcelo', 'Valois'];

    const dadosCliente5 = readonly [number, string, ...string[]] = [1, 'Marcelo', 'Valois'];  // Evita a possibilidade de pop() ou coisas do tipo

# Tipo Never

    A função nunca retorna nada

    export function criaErro(): never {
        throw new Error('Erro qualquer');
    }

    criaErro();

# Tipo Enum

    enum Cores {
        VERMELHO, // 0
        AZUL, // 1
        AMARELO, // 2
    }

    enum Cores {
        VERMELHO, // 0
        AZUL, // 1
        AMARELO, // 2
    }

    * Se criar dois enums com o mesmo nome, o Typescript unifica.

# Tipo unknown

    Mais seguro que o any.

    let x;
    x = '10';
    const y = 800;
    console.log(x + y) // '10800'. O unknown não permite essa operação

# Union Types

    Uma variável pode ter mais de um tipo.
    Ex.:
        a: number | string | boolean

# Type alias

    Podemos criar um apelido para um tipo para deixar o código mais limpo

    type Idade = number;
    type Pessoa = {
        nome: string;
        idade: Idade;
        salario: number;
        corPreferida?: string;
    }

    type CorRGB = 'Vermelho' | 'Azul' | 'Verde';
    type CorCMYK = 'Ciano' | 'Magenta' | 'Amarelo' | 'Preto';
    type CorPreferida = CorRGB | CorCMYK;    // 'Ciano' | 'Magenta' | 'Amarelo' | 'Preto' | 'Vermelho' | 'Azul' | 'Verde'

# Intersection types

    Notação &

    type TemNome = { nome: string };
    type TemSobrenome = { sobrenome: string };
    type TemIdade = { idade: string };
    type Pessoa = TemNome & TemSobrenome & TemIdade;


    type AB = 'A' | 'B';
    type AC = 'A' | 'C';
    type Intersecao = AB & AC; // 'A'

# Funções como tipo

    type MapStringsCallback = (item: string) => string;


# Structural Type System

    interface Ponto2D {
    x: number;
    y: number;
    }

    interface Ponto3D {
    x: number;
    y: number;
    z: number;
    }

    function imprimirPonto2D(ponto: Ponto2D) {
    console.log(`Coordenadas: (${ponto.x}, ${ponto.y})`);
    }

    const ponto3D: Ponto3D = { x: 10, y: 20, z: 30 };

    imprimirPonto2D(ponto3D); // Válido, mesmo com tipos diferentes


    * Em resumo, o Typescript não se importa com a identidade das coisas. Um objeto só precisa cumprir os requisitos mínimos.
    * No exemplo de cima, o ponto3D pode ser enviado e encaixado no ponto2D, porque o ponto2D precisa de x e y de maneira obrigatória, mas não se
        importa se houver um 'z' ou qualquer outro atributo.

# Type assertions

    const body = document.querySelector('body');
    body.style.backgroundColor = 'red';

    * Isso pode dar problema porque o body pode ser nulo.

    Como resolver?

    Duas maneiras:
        1.
            const body = document.querySelector('body');
            if (body) body.style.backgroundColor = 'red';

        2.
            // Type assertion
            const body = document.querySelector('body') as HTMLBodyElement;
            body.style.backgroundColor = 'red';
