# Validações para Português (BR) - Laravel +6

![Travis (.com)](https://img.shields.io/travis/com/consultaazul/pt-br-validator?style=for-the-badge) ![GitHub](https://img.shields.io/github/license/consultaazul/pt-br-validator?style=for-the-badge) ![Packagist Downloads](https://img.shields.io/packagist/dt/consultaazul/pt-br-validator?color=%23248afd&logo=Consulta%20Azul&style=for-the-badge)

Esta é uma biblioteca com validações de dados brasileiros.

## Instalação

Navegue até a pasta do seu projeto e então execute:

```
composer require consultaazul/pt-br-validator
```

Agora, para utilizar a validação, você pode criar manualmente uma instância para o método `Validator::make` utilizando o facade `Illuminate\Support\Facades\Validator`
É possível usar os seguintes métodos de validação:

Validador | Formato do Dado | Tipo de validação
:-------- | :-------------- | :----------------
`celular` | `99999-9999` ou `9999-9999` | Formatação
`celular_com_ddd` | `(99)99999-9999` ou `(99)9999-9999` ou `(99) 99999-9999` ou `(99) 9999-9999` | Formatação
`telefone` | `9999-9999` | Formatação
`telefone_com_ddd` | `(99)9999-9999` ou `(99) 9999-9999` | Formatação
`cnpj` | `99.999.999/9999-99` ou `99999999999999` | Numérica
`formato_cnpj` | `99.999.999/9999-99` ou `99999999999999` | Formatação
`cpf` | `999.999.999-99` ou `99999999999999` | Numérica
`formato_cpf` | `999.999.999-99` | Formatação
`formato_cep` | `99999-999` ou `99.999999-99` | Formatação
`formato_placa_de_veiculo` | `ABC-9999` ou `ABC9999` | Formatação

Você pode utilizá-lo também com a instância de `Illuminate\Http\Request`, através do método `validate`.

~~~php

use Illuminate\Http\Request;

// URL: /testando?telefone=3455-1222

Route::get('testando', function (Request $request) {

    try {

        $dados = $request->validate([
            'telefone' => 'required|telefone',
            // Outras validações
        ]);

    } catch (\Illuminate\Validation\ValidationException $e) {
        dd($e->errors());
    }
});
~~~

### Dicas:

É possível gerar números de CPF ou CNPJ válido para seus testes utilizando o site `4devs.com.br` [Gerador de CNPJ](https://www.4devs.com.br/gerador_de_cnpj) | [Gerador de CPF](https://www.4devs.com.br/gerador_de_cpf)

### Testando

Com isso, é possível fazer um teste simples


~~~php
$validator = Validator::make(
    ['telefone' => '(77)9999-3333'],
    ['telefone' => 'required|telefone_com_ddd']
);

dd($validator->fails());
~~~

### Customizando as mensagens

Todas as validações citadas acima já contam mensagens padrões de validação, porém, é possível alterar isto usando o terceiro parâmetro de `Validator::make`. Este parâmetro deve ser um array onde os índices sejam os nomes das validações e os valores devem ser as respectivas mensagens.

Por exemplo:

~~~php
Validator::make(
    ['celular_com_ddd' => '99999-9999'], // valor 
    ['celular_com_ddd' => 'required|telefone_com_ddd'], // regra
    ['celular_com_ddd' => 'O campo :attribute não é um celular com ddd'] // mensagem
);
~~~

Ou através do método `messages` do seu Request criado pelo comando `php artisan make:request`.

~~~php
public function messages() {
    return [
        'campo.telefone' => 'Telefone não válido!'
    ];
}
~~~
