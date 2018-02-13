# Criptografia

- [Introdução](#introduction)
- [Configuração](#configuration)
- [Como Usar](#using-the-encrypter)

<a name="introduction"></a>
## Introdução

O criptografador do Laravel usa o OpenSSL para fornecer criptografia AES-256 e AES-128. O ideal é que você use a criptografia do Laravel e não tente criar seus próprios algoritmos de criptografia "caseiros". Todos os dados criptografados da Laravel são assinados usando um código de autenticação de mensagens (MAC) para que, depois de criptografado, seu valor não possa ser modificado.

<a name="configuration"></a>
## Configuração

Before using Laravel's encrypter, you must set a `key` option in your `config/app.php` configuration file. You should use the `php artisan key:generate` command to generate this key since this Artisan command will use PHP's secure random bytes generator to build your key. If this value is not properly set, all values encrypted by Laravel will be insecure.

Antes de usar o criptografador do Laravel, você deve definir uma `key` no seu arquivo de configuração `config/app.php`. Utilize o comando `php artisan key:generate` para gerar essa chave. Este comando usa um gerador de bytes aleatório do PHP para criar sua chave. Se este valor não for definido corretamente, todos os valores criptografados pelo Laravel serão inseguros.

<a name="using-the-encrypter"></a>
## Como Usar

#### Criptografando um Valor

Você pode criptografar um valor usando o helper `encrypt`. Todos os valores são criptografados usando OpenSSL e a cifra `AES-256-CBC`. Além disso, todos os valores criptografados são assinados com um código de autenticação de mensagens (MAC) para detectar quaisquer modificações na sequência criptografada:

    <?php

    namespace App\Http\Controllers;

    use App\User;
    use Illuminate\Http\Request;
    use App\Http\Controllers\Controller;

    class UserController extends Controller
    {
        /**
         * Store a secret message for the user.
         *
         * @param  Request  $request
         * @param  int  $id
         * @return Response
         */
        public function storeSecret(Request $request, $id)
        {
            $user = User::findOrFail($id);

            $user->fill([
                'secret' => encrypt($request->secret)
            ])->save();
        }
    }

#### Criptografando sem serialização

Os valores criptografados são passados através de serialização durante a criptografia, o que permite criptografia de objetos e matrizes. Assim, clientes não-PHP que recebem valores criptografados precisarão desmarcar os dados. Se você quiser criptografar e descriptografar valores sem serialização, você pode usar os métodos `encryptString` e `decryptString` da classe `Crypt`, dentro de `facade`:

    use Illuminate\Support\Facades\Crypt;

    $encrypted = Crypt::encryptString('Hello world.');

    $decrypted = Crypt::decryptString($encrypted);

#### Descriptografando um Valor

Você pode descriptografar valores usando o helper `decrypt`. Se o valor não puder ser descriptografado corretamente, quando, por exemplo, o MAC for inválido, uma exceção `Illuminate\Contracts\Encryption\DecryptException` será lançada:

    use Illuminate\Contracts\Encryption\DecryptException;

    try {
        $decrypted = decrypt($encryptedValue);
    } catch (DecryptException $e) {
        //
    }
