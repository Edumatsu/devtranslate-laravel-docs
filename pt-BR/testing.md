# Testes

- [Introdução](#introduction)
- [Ambiente](#environment)
- [Criando & Executando Testes](#creating-and-running-tests)

<a name="introduction"></a>
## Introdução

O Laravel já foi construído pensando nos testes. Na verdade, o suporte para testes com PHPUnit está incluído no pacote e um arquivo `phpunit.xml` já está configurado para sua aplicação. O framework também fornece métodos auxiliares que permitem testar expressivamente suas aplicações.

Por padrão, o diretório `tests` do seu aplicativo contém dois diretórios: `Feature` e `Unit`. Testes unitários são testes que se concentram em um pedaço muito pequeno e isolado do seu código. Na verdade, a maioria dos testes unitários provavelmente se concentra em um único método. Os testes de recurso podem testar um pedaço maior de seu código, incluindo a forma como vários objetos interagem uns com os outros ou mesmo uma solicitação HTTP completa para um endpoint JSON.

Existe um arquivo chamado `ExampleTest.php` nos dois diretórios de teste: `Feature` e `Unit`. Depois de instalar o Laravel, basta executar `phpunit` na linha de comando para rodar seus testes.

<a name="environment"></a>
## Ambiente

Ao executar testes via `phpunit`, o Laravel irá criar automaticamente o ambiente de configuração para teste seguindo às variáveis de ambiente definidas no arquivo `phpunit.xml`. O Laravel também configura automaticamente a sessão e o cache durante o teste, o que significa que nenhuma sessão ou dados de cache serão persistidos durante o teste.

Você é livre para definir outros valores de configuração do ambiente de teste conforme necessário. As variáveis de ambiente de teste podem ser configuradas no arquivo `phpunit.xml`, porém, certifique-se de limpar seu cache de configuração usando o comando do Artisan `config:clear` antes de executar seus testes!

<a name="creating-and-running-tests"></a>
## Criando & Executando Testes

Para criar um novo caso de teste, o comando do Artisan `make:test`:

    // Criando um teste no diretório Feature ...
    php artisan make:test UserTest

    // Criando um teste no diretório Unit ...
    php artisan make:test UserTest --unit

Once the test has been generated, you may define test methods as you normally would using PHPUnit. To run your tests, simply execute the `phpunit` command from your terminal:

Uma vez que o teste foi gerado, você pode definir métodos de teste da mesma forma que você definiria no PHPUnit. Para executar seus testes, basta executar o comando `phpunit` no seu terminal:

    <?php

    namespace Tests\Unit;

    use Tests\TestCase;
    use Illuminate\Foundation\Testing\RefreshDatabase;

    class ExampleTest extends TestCase
    {
        /**
         * A basic test example.
         *
         * @return void
         */
        public function testBasicTest()
        {
            $this->assertTrue(true);
        }
    }

> {nota} Se você definir seu próprio método `setUp` dentro de uma classe de teste, não se esqueça de chamar `parent::setUp()`.